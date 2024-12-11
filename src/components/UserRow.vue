<template>
  <tr>
    <td :style="{ paddingLeft: `${level * 20}px` }">
      <span
          v-if="user.children.length"
          @click.stop="toggle(user.id)"
          class="toggle-icon"
      >
        {{ user.expanded ? '▼' : '▶' }}
      </span>
      {{ user.name || 'Нет имени' }}
    </td>
    <td>{{ user.phone || 'Нет телефона' }}</td>
  </tr>
  <template v-if="user.expanded">
    <UserRow
        v-for="child in user.children"
        :key="child.id"
        :user="child"
        :level="level + 1"
        @toggle="$emit('toggle', $event)"
    />
  </template>
</template>

<script>
export default {
  props: ['user', 'level'],
  methods: {
    toggle(id) {
      this.$emit('toggle', id);
    },
  },
};
</script>

<style scoped>
.toggle-icon {
  cursor: pointer;
  margin-right: 5px;
}
</style>
