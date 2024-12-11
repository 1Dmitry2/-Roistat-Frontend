<template>
  <div>
    <button class="btn-add-user" @click="openModal">Добавить пользователя</button>
    <UserTable :users="users" @toggle="toggleUser" />
    <UserFormModal
        :visible="modalVisible"
        :users="flatUsers"
        @save="addUser"
        @close="closeModal"
    />
  </div>
</template>

<script>
import UserTable from './components/UserTable.vue';
import UserFormModal from './components/UserFormModal.vue';

export default {
  components: { UserTable, UserFormModal },
  data() {
    return {
      users: JSON.parse(localStorage.getItem('users')) || [],
      modalVisible: false,
    };
  },
  computed: {
    flatUsers() {
      const flatten = (users, level = 0) =>
          users.reduce((acc, user) => {
            acc.push({ ...user, level });
            return acc.concat(flatten(user.children, level + 1));
          }, []);
      return flatten(this.users);
    },
  },
  methods: {
    openModal() {
      this.modalVisible = true;
    },
    closeModal() {
      this.modalVisible = false;
    },
    addUser(newUser) {
      const addToTree = (users, parentId) => {
        if (parentId === null) {
          users.push(newUser);
        } else {
          users.forEach((user) => {
            if (user.id === parentId) {
              user.children.push(newUser);
            } else {
              addToTree(user.children, parentId);
            }
          });
        }
      };
      addToTree(this.users, newUser.parentId);
      this.saveUsers();
      this.closeModal();
    },
    saveUsers() {
      localStorage.setItem('users', JSON.stringify(this.users));
    },
    toggleUser(id) {
      const toggleInTree = (users) => {
        users.forEach((user) => {
          if (user.id === id) {
            user.expanded = !user.expanded;
          } else {
            toggleInTree(user.children);
          }
        });
      };
      toggleInTree(this.users);
    },
  },
};
</script>

<style scoped>
body {
  font-family: 'Arial', sans-serif;
  margin: 0;
  padding: 0;
}

.btn-add-user {
  margin: 20px;
  padding: 10px 20px;
  background-color: #007bff;
  color: white;
  border: none;
  cursor: pointer;
  border-radius: 5px;
  font-size: 16px;
  transition: transform 0.2s, background-color 0.2s;
}

.btn-add-user:hover {
  background-color: #0056b3;
  transform: scale(1.05);
}
</style>
