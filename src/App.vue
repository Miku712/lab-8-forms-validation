<script setup>
import RegistrationForm from './components/RegistrationForm.vue'
</script>

<template>
  <div class="app">
    <h1>Реєстрація</h1>
    <RegistrationForm />
  </div>
</template>

<style scoped>
.app {
  max-width: 500px;
  margin: auto;
  padding: 20px;
  font-family: 'Segoe UI', sans-serif;
}
h1 {
  color: #0369a1;
  text-align: center;
}
</style>