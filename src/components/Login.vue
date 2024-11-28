<template>
    <div class="container">
      <h2>Вход</h2>
      <form @submit.prevent="login">
        <div class="form-group">
          <input v-model="username" placeholder="Логин" required />
        </div>
        <div class="form-group">
          <input v-model="password" type="password" placeholder="Пароль" required />
        </div>
        <button type="submit">Войти</button>
      </form>
      <p v-if="errorMessage" class="error-message">{{ errorMessage }}</p>
    </div>
  </template>
  
  <script>
  export default {
    name: 'UserLogin', 
    data() {
      return {
        username: '',
        password: '',
        errorMessage: ''
      };
    },
    methods: {
      async login() {
        try {
          const response = await this.$http.get(`/users?username=${this.username}&password=${this.password}`);
          if (response.data.length === 0) {
            this.errorMessage = 'Неверный логин или пароль';
            return;
          }
          // Успешная авторизация
          this.$router.push('/');
        } catch (error) {
          this.errorMessage = 'Ошибка авторизации';
        }
      }
    }
  };
  </script>

