<template>
    <div class="home-container">
      <h2>Смартфоны</h2>
      <table>
        <thead>
          <tr>
            <th>Марка</th>
            <th>Модель</th>
            <th>Цена (евро)</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="phone in phones" :key="phone.id">
            <td>{{ phone.brand }}</td>
            <td>{{ phone.model }}</td>
            <td>{{ phone.price }}</td>
          </tr>
        </tbody>
      </table>
  
      <h3>Добавить новый смартфон</h3>
      <form @submit.prevent="addPhone">
        <div class="form-group">
          <input v-model="newPhone.brand" placeholder="Марка" required />
        </div>
        <div class="form-group">
          <input v-model="newPhone.model" placeholder="Модель" required />
        </div>
        <div class="form-group">
          <input type="number" v-model="newPhone.price" placeholder="Цена (евро)" required min="0" />
        </div>
        <button type="submit">Добавить смартфон</button>
      </form>
  
      <p v-if="errorMessage" class="error-message">{{ errorMessage }}</p>
    </div>
  </template>
  
  <script>
  export default {
    name: 'Home',
    data() {
      return {
        phones: [],
        newPhone: {
          brand: '',
          model: '',
          price: null,
        },
        errorMessage: '',
      };
    },
    created() {
      this.fetchPhones();
    },
    methods: {
      async fetchPhones() {
        try {
          const response = await this.$http.get('/phones');
          this.phones = response.data;
        } catch (error) {
          console.error('Ошибка при загрузке смартфонов:', error);
        }
      },
      async addPhone() {
        // Проверка на существующий смартфон
        const exists = this.phones.some(phone => 
          phone.brand === this.newPhone.brand && phone.model === this.newPhone.model
        );
  
        if (exists) {
          this.errorMessage = 'Смартфон с такой маркой и моделью уже существует!';
          return;
        }
  
        try {
          const response = await this.$http.post('/phones', this.newPhone);
          this.phones.push(response.data); // Добавление нового смартфона в список
          this.newPhone.brand = '';
          this.newPhone.model = '';
          this.newPhone.price = null;
          this.errorMessage = ''; // Сброс сообщения об ошибке
        } catch (error) {
          console.error('Ошибка при добавлении смартфона:', error);
          this.errorMessage = 'Не удалось добавить смартфон.';
        }
      }
    }
  };
  </script>
  
  <style scoped>
  .home-container {
    max-width: 600px;
    margin: auto;
  }
  
  table {
    width: 100%;
    border-collapse: collapse;
  }
  
  th, td {
    padding: 10px;
    border: 1px solid #ccc;
  }
  
  .form-group {
    margin-bottom: 15px;
  }
  
  input {
    width: calc(100% - 20px);
    padding: 10px;
  }
  
  button {
    width: 100%;
    padding: 10px;
    background-color: #007bff;
    color: white;
    border: none;
    border-radius: 4px;
  }
  
  button:hover {
    background-color: #0056b3;
  }
  
  .error-message {
    color: red;
  }
  </style>