<template>
  <section class="tasksContainer">
    <div class="login-page">
      <div class="form-header">
        <h2>Создание аккаунта</h2>
        <p>Заполните данные для регистрации</p>
      </div>

      <form class="register-form" @submit.prevent="onSubmit" novalidate>
        <div class="input-container">
          <input
              class="styled-input"
              type="email"
              v-model="email"
              @blur="touchedEmail = true"
              placeholder="Введите ваш email"
              aria-label="Email"
              required
          />
          <span class="input-icon">📧</span>
        </div>

        <p v-if="touchedEmail && !isValidEmail" class="validation-error">
          ⚠️ Некорректный E-mail
        </p>

        <div class="input-container">
          <input
              class="styled-input"
              type="password"
              v-model="password"
              @blur="touchedPassword = true"
              placeholder="Придумайте пароль"
              aria-label="Пароль"
              required
              minlength="6"
          />
          <span class="input-icon">🔒</span>
        </div>

        <div class="input-container">
          <input
              class="styled-input"
              type="password"
              v-model="confirmPassword"
              @blur="touchedConfirm = true"
              placeholder="Повторите пароль"
              aria-label="Подтверждение пароля"
              required
              minlength="6"
          />
          <span class="input-icon">✅</span>
        </div>

        <p v-if="touchedConfirm && !passwordsMatch" class="validation-error">
          ⚠️ Пароли не совпадают
        </p>

        <button
            id="register"
            class="styled-button"
            type="submit"
            :disabled="!canSubmit"
            :class="{ 'button-disabled': !canSubmit }"
        >
          <span class="button-content">
            {{ submitLabel }}
            <span class="button-arrow">→</span>
          </span>
        </button>
      </form>
    </div>
  </section>
</template>

<script setup>
import { useRouter } from 'vue-router'
const router = useRouter()

import { ref, computed } from 'vue';
import { useUserStore } from '@/stores/useUserStore.js'

const store = useUserStore()
const email = ref('');
const password = ref('');
const confirmPassword = ref('');

const touchedEmail = ref(false);
const touchedPassword = ref(false);
const touchedConfirm = ref(false);

const submitLabel = 'Зарегистрироваться';

const emailRegex = /^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$/;
const isValidEmail = computed(() => emailRegex.test(email.value));

const passwordsMatch = computed(() => password.value !== '' && password.value === confirmPassword.value);

const canSubmit = computed(() => isValidEmail.value && passwordsMatch.value);

function onSubmit() {

  touchedEmail.value = true;
  touchedPassword.value = true;
  touchedConfirm.value = true;

  if (!canSubmit.value) return;
  store.registerUser(email.value, password.value)

  console.log('Register:', { email: email.value, password: password.value });

  email.value = '';
  password.value = '';
  confirmPassword.value = '';
  touchedEmail.value = false;
  touchedPassword.value = false;
  touchedConfirm.value = false;
  router.push('/todos')
}
</script>

<style scoped>
.tasksContainer {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 80vh;
  padding: 20px;
}

.login-page {
  width: 100%;
  max-width: 440px;
  background: var(--color-background);
  padding: 40px 80px;
  border-radius: 20px;
  box-shadow: 0 15px 35px rgba(0, 0, 0, 0.1);
  border: 1px solid var(--color-border);
  backdrop-filter: blur(10px);
}

.form-header {
  text-align: center;
  margin-bottom: 30px;
}

.form-header h2 {
  color: rgba(44, 79, 229, 0.75);
  font-size: 1.8rem;
  font-weight: 700;
  margin-bottom: 8px;
}

.form-header p {
  color: #718096;
  font-size: 0.95rem;
}

.register-form {
  display: flex;
  flex-direction: column;
  gap: 15px;
}

.input-container {
  position: relative;
}

.styled-input {
  width: 100%;
  padding: 14px 45px 14px 16px;
  border: 2px solid #ffffff;
  border-radius: 10px;
  font-size: 1rem;
  background: #f7fafc;
  transition: all 0.3s ease;
  color: #2d3748;
}

.styled-input:focus {
  outline: none;
  border-color: #4299e1;
  background: white;
  box-shadow: 0 0 0 3px rgba(66, 153, 225, 0.1);
}

.styled-input::placeholder {
  color: #a0aec0;
}

.input-icon {
  position: absolute;
  right: 16px;
  top: 50%;
  transform: translateY(-50%);
  font-size: 1.1rem;
  color: #a0aec0;
}

.validation-error {
  color: #e53e3e;
  font-size: 0.85rem;
  margin: 5px 0 0 0;
  padding-left: 5px;
  display: flex;
  align-items: center;
  gap: 6px;
}

.styled-button {
  width: 100%;
  padding: 16px 24px;
  background: #4299e1;
  color: white;
  border: none;
  border-radius: 10px;
  font-size: 1rem;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.3s ease;
  margin-top: 10px;
}

.styled-button:hover:not(.button-disabled) {
  transform: translateY(-2px);
  box-shadow: 0 8px 20px rgba(66, 153, 225, 0.3);
  background: #3182ce;
}

.styled-button:active:not(.button-disabled) {
  transform: translateY(0);
}

.button-disabled {
  opacity: 0.6;
  cursor: not-allowed;
  background: #cbd5e0 !important;
}

.button-disabled:hover {
  transform: none !important;
  box-shadow: none !important;
}

.button-content {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
}

.button-arrow {
  transition: transform 0.3s ease;
}

.styled-button:hover:not(.button-disabled) .button-arrow {
  transform: translateX(3px);
}

@media (max-width: 480px) {
  .login-page {
    padding: 30px 20px;
    margin: 0 10px;
  }

  .form-header h2 {
    font-size: 1.5rem;
  }
}
</style>