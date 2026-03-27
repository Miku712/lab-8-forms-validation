<script setup>
import { reactive, ref, watch } from 'vue'

const form = reactive({
  name: '',
  email: '',
  password: '',
  confirmPassword: '',
  age: null,
  agree: false
})

const errors = reactive({})
const isSubmitting = ref(false)
const success = ref(false)

function validate() {
  Object.keys(errors).forEach(k => delete errors[k])

  if (!form.name) errors.name = "Обов'язкове поле"
  if (!form.email.includes('@')) errors.email = "Невірний email"
  if (form.password.length < 6) errors.password = "Мінімум 6 символів"
  if (form.password !== form.confirmPassword)
    errors.confirmPassword = "Паролі не співпадають"
  if (form.age < 18) errors.age = "18+"
  if (!form.agree) errors.agree = "Потрібно погодитись"

  return Object.keys(errors).length === 0
}

watch(form, () => {
  Object.keys(errors).forEach(k => delete errors[k])
})

function submitForm() {
  if (!validate()) return

  isSubmitting.value = true

  setTimeout(() => {
    success.value = true
    isSubmitting.value = false

    Object.keys(form).forEach(k => form[k] = k === 'agree' ? false : '')
  }, 1500)
}
</script>

<template>
  <form @submit.prevent="submitForm">
    <input v-model="form.name" placeholder="Ім'я" />
    <p v-if="errors.name">{{ errors.name }}</p>

    <input v-model="form.email" placeholder="Email" />
    <p v-if="errors.email">{{ errors.email }}</p>

    <input v-model="form.password" type="password" placeholder="Пароль" />
    <p v-if="errors.password">{{ errors.password }}</p>

    <input v-model="form.confirmPassword" type="password" placeholder="Підтвердження" />
    <p v-if="errors.confirmPassword">{{ errors.confirmPassword }}</p>

    <input v-model.number="form.age" type="number" placeholder="Вік" />
    <p v-if="errors.age">{{ errors.age }}</p>

    <label>
      <input type="checkbox" v-model="form.agree" />
      Погоджуюсь
    </label>
    <p v-if="errors.agree">{{ errors.agree }}</p>

    <button :disabled="isSubmitting">
      {{ isSubmitting ? "Відправка..." : "Зареєструватися" }}
    </button>

    <p v-if="success">Успішно!</p>
  </form>
</template>