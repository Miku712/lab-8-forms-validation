<script setup>
import { reactive } from 'vue'

const form = reactive({
  name: '',
  email: '',
  password: '',
  confirmPassword: '',
  age: null,
  agree: false
})

function submitForm() {
  console.log(form)
}
</script>

<template>
  <form @submit.prevent="submitForm">
    <input v-model.trim="form.name" placeholder="Ім'я" />
    <input v-model.trim="form.email" type="email" placeholder="Email" />
    <input v-model="form.password" type="password" placeholder="Пароль" />
    <input v-model="form.confirmPassword" type="password" placeholder="Підтвердження" />
    <input v-model.number="form.age" type="number" placeholder="Вік" />

    <label>
      <input type="checkbox" v-model="form.agree" />
      Погоджуюсь
    </label>

    <button type="submit">Зареєструватися</button>
  </form>
</template>