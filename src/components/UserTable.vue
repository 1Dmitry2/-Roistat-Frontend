<template>
  <table class="user-table">
    <thead>
    <tr>
      <th @click="sort('name')">Имя</th>
      <th @click="sort('phone')">Телефон</th>
    </tr>
    </thead>
    <tbody>
    <template v-for="user in sortedUsers" :key="user.id">
      <UserRow :user="user" :level="0" @toggle="toggle" />
    </template>
    </tbody>
  </table>
</template>

<script>
import UserRow from './UserRow.vue';

export default {
  props: ['users'],
  components: { UserRow },
  data() {
    return {
      sortKey: 'name',
      sortOrder: 'asc',
    };
  },
  computed: {
    sortedUsers() {
      const sortUsers = (users) =>
          [...users]
              .sort((a, b) => {
                const aValue = a[this.sortKey] || '';
                const bValue = b[this.sortKey] || '';
                const compare = aValue.localeCompare(bValue);
                return this.sortOrder === 'asc' ? compare : -compare;
              })
              .map((user) => ({
                ...user,
                children: sortUsers(user.children),
              }));
      return sortUsers(this.users);
    },
  },
  methods: {
    sort(key) {
      this.sortKey = key;
      this.sortOrder = this.sortOrder === 'asc' ? 'desc' : 'asc';
    },
    toggle(id) {
      this.$emit('toggle', id);
    },
  },
};
</script>

<style scoped>
.user-table {
  width: 100%;
  border-collapse: collapse;
  background-color: white;
  margin: 20px 0;
  border-radius: 5px;
  overflow: hidden;
}

thead th {
  padding: 10px;
  background-color: #959595;
  color: white;
  cursor: pointer;
}

tbody td {
  padding: 10px;
  border-bottom: 1px solid #ddd;
}

tbody tr:hover {
  background-color: #f1f1f1;
}
</style>
