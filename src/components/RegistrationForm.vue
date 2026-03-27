<script setup>
import { reactive, ref } from 'vue'

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
  if (!form.age || form.age < 18) errors.age = "Мінімум 18 років"
  if (!form.agree) errors.agree = "Потрібно погодитись"

  return Object.keys(errors).length === 0
}

function submitForm() {
  success.value = false

  if (!validate()) return

  isSubmitting.value = true

  setTimeout(() => {
    isSubmitting.value = false
    success.value = true
  }, 1500)
}
</script>

<template>
  <form @submit.prevent="submitForm" class="form">

    <input v-model="form.name" placeholder="Ім'я" />
    <Transition name="fade">
      <p v-if="errors.name" class="error">{{ errors.name }}</p>
    </Transition>

    <input v-model="form.email" placeholder="Email" />
    <Transition name="fade">
      <p v-if="errors.email" class="error">{{ errors.email }}</p>
    </Transition>

    <input v-model="form.password" type="password" placeholder="Пароль" />
    <Transition name="fade">
      <p v-if="errors.password" class="error">{{ errors.password }}</p>
    </Transition>

    <input v-model="form.confirmPassword" type="password" placeholder="Підтвердження" />
    <Transition name="fade">
      <p v-if="errors.confirmPassword" class="error">
        {{ errors.confirmPassword }}
      </p>
    </Transition>

    <input v-model.number="form.age" type="number" placeholder="Вік" />
    <Transition name="fade">
      <p v-if="errors.age" class="error">{{ errors.age }}</p>
    </Transition>

    <label>
      <input type="checkbox" v-model="form.agree" />
      Погоджуюсь з умовами
    </label>
    <Transition name="fade">
      <p v-if="errors.agree" class="error">{{ errors.agree }}</p>
    </Transition>

    <button :disabled="isSubmitting">
      {{ isSubmitting ? "Відправка..." : "Зареєструватися" }}
    </button>

    <Transition name="fade">
      <p v-if="success" class="success">
        Успішна реєстрація!
      </p>
    </Transition>

  </form>
</template>

<style scoped>
.form {
  display: flex;
  flex-direction: column;
  gap: 10px;
}

.error {
  color: red;
  font-size: 0.9rem;
}

.success {
  color: green;
  margin-top: 10px;
}

.fade-enter-active,
.fade-leave-active {
  transition: all 0.3s ease;
}

.fade-enter-from {
  opacity: 0;
  transform: translateY(-5px);
}

.fade-leave-to {
  opacity: 0;
  transform: translateY(-5px);
}
</style>