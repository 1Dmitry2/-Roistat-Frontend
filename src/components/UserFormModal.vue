<template>
  <div v-if="visible" class="modal-overlay">
    <div class="modal">
      <h3>Добавить пользователя</h3>
      <form @submit.prevent="saveUser">
        <label>
          Имя:
          <input type="text" v-model="name" required />
        </label>
        <label>
          Телефон:
          <input type="tel" v-model="phone" required />
        </label>
        <label>
          Начальник:
          <select v-model="parentId">
            <option :value="null">Без начальника</option>
            <option v-for="user in users" :key="user.id" :value="user.id">
              {{ user.name }}
            </option>
          </select>
        </label>
        <div class="form-actions">
          <button type="button" @click="$emit('close')">Отмена</button>
          <button type="submit">Сохранить</button>
        </div>
      </form>
    </div>
  </div>
</template>

<script>
export default {
  props: ['visible', 'users'],
  data() {
    return {
      name: '',
      phone: '',
      parentId: null,
    };
  },
  methods: {
    saveUser() {
      const newUser = {
        id: Date.now(),
        name: this.name,
        phone: this.phone,
        parentId: this.parentId,
        children: [],
      };
      this.$emit('save', newUser);
      this.resetForm();
    },
    resetForm() {
      this.name = '';
      this.phone = '';
      this.parentId = null;
    },
  },
};
</script>

<style scoped>
.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.6);
  display: flex;
  align-items: center;
  justify-content: center;
  animation: fadeIn 0.3s;
}

@keyframes fadeIn {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}

.modal {
  background: white;
  padding: 20px;
  border-radius: 10px;
  width: 350px;
  box-shadow: 0 5px 15px rgba(0, 0, 0, 0.3);
  transform: scale(0.95);
  animation: scaleUp 0.3s;
}

@keyframes scaleUp {
  from {
    transform: scale(0.9);
  }
  to {
    transform: scale(1);
  }
}

label {
  display: block;
  margin-bottom: 10px;
}

input,
select {
  width: 100%;
  padding: 8px;
  margin-top: 5px;
  border: 1px solid #ddd;
  border-radius: 4px;
}

button {
  padding: 10px 15px;
  background-color: #007bff;
  color: white;
  border: none;
  cursor: pointer;
  border-radius: 5px;
  transition: transform 0.2s, background-color 0.2s;
}

button:hover {
  background-color: #0056b3;
  transform: scale(1.05);
}
</style>
