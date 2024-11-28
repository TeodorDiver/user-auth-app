<template>
  <div id="app">
    <nav>
      <ul>
      <li><router-link to="/register">Регистрация</router-link></li>
      <br>
      <li><router-link to="/login">Войти</router-link></li>
    </ul>
    </nav>
    <router-view></router-view>
  </div>
</template>

<script>
export default {
  name: 'App',
}
</script>


