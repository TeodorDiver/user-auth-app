<template>
  <div class="container">
    <h2>Регистрация</h2>
    <form @submit.prevent="register">
      <div class="form-group">
        <input v-model="username" placeholder="Логин" required />
      </div>
      <div class="form-group">
        <input v-model="password" type="password" placeholder="Пароль" required />
      </div>
      <div class="form-group">
        <input v-model="confirmPassword" type="password" placeholder="Подтвердите пароль" required />
      </div>
      <div class="form-group">
        <input v-model="name" placeholder="Имя" required />
      </div>
      <div class="check-container">
           <label for="agreement">Принять условия соглашения и...</label>
         </div>
      <button type="submit">Зарегистрироваться</button>
    </form>
    <p v-if="errorMessage" class="error-message">{{ errorMessage }}</p>
  </div>
</template>

<script>
export default {
  name: 'UserRegister', 
  data() {
    return {
      username: '',
      password: '',
      confirmPassword: '',
      name: '',
      agreement: false,
      errorMessage: ''
    };
  },
  methods: {
    async register() {
      if (this.password !== this.confirmPassword) {
        this.errorMessage = 'Пароли не совпадают';
        return;
      }
      if (!/^[а-яА-ЯёЁ\s]+$/.test(this.name)) {
        this.errorMessage = 'Имя должно содержать только русские буквы';
        return;
      }

      try {
        const response = await this.$http.get(`/users?username=${this.username}`);
        if (response.data.length > 0) {
          this.errorMessage = 'Логин уже существует, выберите другой';
          return;
        }

        await this.$http.post('/users', {
          username: this.username,
          password: this.password,
          name: this.name
        });
        this.$router.push('/login');
      } catch (error) {
        this.errorMessage = 'Ошибка регистрации';
      }
    }
  }
};
</script>
<style src="../styles.css"></style>
