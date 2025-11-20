<template>
  <section class="login-section">
    <div class="login-container">
      <div class="login-header">
        <h1 class="login-title">Вход в систему</h1>
        <p class="login-subtitle">Добро пожаловать обратно!</p>
      </div>

      <form class="login-form" @submit.prevent="onSubmit" novalidate>
        <div class="input-group">
          <input
              class="login-input"
              type="email"
              v-model="email"
              @blur="touchedEmail = true"
              placeholder=" "
              aria-label="Email"
              required
          />
          <label class="input-label">Email</label>
          <span class="input-icon">✉️</span>
        </div>

        <p v-if="touchedEmail && !isValidEmail" class="validation-error">
          ⚠️ Некорректный E-mail
        </p>

        <div class="input-group">
          <input
              class="login-input"
              type="password"
              v-model="password"
              @blur="touchedPassword = true"
              placeholder=" "
              aria-label="Пароль"
              required
              minlength="6"
          />
          <label class="input-label">Пароль</label>
          <span class="input-icon">🔒</span>
        </div>

        <p v-if="errorMessage" id="error" class="validation-error validation-error-main">
          ❌ {{ errorMessage }}
        </p>

        <button
            id="register"
            class="login-button"
            type="submit"
            :disabled="!canSubmit"
            :class="{ 'button-disabled': !canSubmit }"
        >
          <span class="button-text">{{ submitLabel }}</span>
          <span class="button-icon">→</span>
        </button>
      </form>

      <div class="login-footer">
        <p class="footer-text">
          Нет аккаунта?
          <router-link to="/register" class="footer-link">Зарегистрируйтесь</router-link>
        </p>
      </div>
    </div>
  </section>
</template>

<script setup>
import { ref, computed } from 'vue';
import { useUserStore } from '@/stores/useUserStore.js'
import { useRouter } from 'vue-router'

const router = useRouter()
const store = useUserStore()
const email = ref('');
const password = ref('');
const errorMessage = ref('')

const touchedEmail = ref(false);
const touchedPassword = ref(false);

const submitLabel = 'Войти';

const emailRegex = /^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$/;
const isValidEmail = computed(() => emailRegex.test(email.value));

const canSubmit = computed(() => isValidEmail.value);

async function onSubmit() {
  errorMessage.value = ''

  try {
    store.loginUser(email.value, password.value)
  } catch (err) {
    errorMessage.value = err.message
    return
  }

  touchedEmail.value = true
  touchedPassword.value = true

  if (!canSubmit.value) return

  console.log('Log in:', { email: email.value, password: password.value })

  email.value = ''
  password.value = ''

  touchedEmail.value = false
  touchedPassword.value = false
  router.push('/todos')
}
</script>

<style scoped lang="scss">
@use '../scss/utils/variables' as *;
@use '../scss/utils/mixins' as *;

.login-section {
  @include flex-center;
  min-height: 80vh;
  padding: 20px;
  background: linear-gradient(135deg, var(--color-background-soft) 0%, var(--color-background) 100%);
}

.login-container {
  width: 100%;
  max-width: 440px;
  background: var(--color-background);
  padding: 40px 35px;
  border-radius: 20px;
  box-shadow: 0 15px 35px rgba(0, 0, 0, 0.1);
  border: 1px solid var(--color-border);
  backdrop-filter: blur(10px);
}

.login-header {
  text-align: center;
  margin-bottom: 35px;
}

.login-title {
  font-size: 2.2rem;
  font-weight: 700;
  background: linear-gradient(135deg, var(--color-primary) 0%, var(--color-secondary) 100%);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
  margin-bottom: 8px;
}

.login-subtitle {
  color: var(--color-text-soft);
  font-size: 1rem;
  font-weight: 500;
}

.login-form {
  display: flex;
  flex-direction: column;
  gap: 20px;
}

.input-group {
  position: relative;
  margin-bottom: 5px;
}

.login-input {
  width: 100%;
  height: 56px;
  padding: 16px 45px 0 16px;
  font-size: 1rem;
  background: var(--color-background-soft);
  border: 2px solid var(--color-border);
  border-radius: 12px;
  outline: none;
  transition: all 0.3s ease;
  color: var(--color-text);

  &:focus {
    border-color: var(--color-primary);
    background: var(--color-background);
    box-shadow: 0 0 0 3px rgba(100, 108, 255, 0.1);
  }

  &:focus + .input-label,
  &:not(:placeholder-shown) + .input-label {
    transform: translateY(-12px) scale(0.85);
    color: var(--color-primary);
  }
}

.input-label {
  position: absolute;
  top: 18px;
  left: 16px;
  font-size: 1rem;
  color: var(--color-text-soft);
  pointer-events: none;
  transition: all 0.3s ease;
  background: var(--color-background-soft);
  padding: 0 4px;
}

.input-icon {
  position: absolute;
  right: 16px;
  top: 50%;
  transform: translateY(-50%);
  font-size: 1.1rem;
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

.validation-error-main {
  background: rgba(229, 62, 62, 0.1);
  padding: 12px;
  border-radius: 8px;
  border-left: 4px solid #e53e3e;
  margin-top: 10px;
}

.login-button {
  width: 100%;
  height: 56px;
  background: linear-gradient(135deg, var(--color-primary) 0%, var(--color-secondary) 100%);
  color: cornflowerblue;
  border: none;
  border-radius: 12px;
  font-size: 1.1rem;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.3s ease;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 10px;
  margin-top: 10px;

  &:hover:not(.button-disabled) {
    transform: translateY(-2px);
    box-shadow: 0 8px 25px rgba(100, 108, 255, 0.3);
  }

  &:active:not(.button-disabled) {
    transform: translateY(0);
  }
}

.button-disabled {
  opacity: 0.6;
  cursor: not-allowed;
  background: var(--color-border) !important;

  &:hover {
    transform: none !important;
    box-shadow: none !important;
  }
}

.button-text {
  font-weight: 600;
}

.button-icon {
  font-size: 1.2rem;
  transition: transform 0.3s ease;
}

.login-button:hover:not(.button-disabled) .button-icon {
  transform: translateX(3px);
}

.login-footer {
  text-align: center;
  margin-top: 25px;
  padding-top: 20px;
  border-top: 1px solid var(--color-border);
}

.footer-text {
  color: var(--color-text);
  font-size: 0.95rem;
}

.footer-link {
  color: cornflowerblue;;
  text-decoration: none;
  font-weight: 600;
  transition: color 0.3s ease;

  &:hover {
    color: var(--color-secondary);
    text-decoration: underline;
  }
}

@media (max-width: 480px) {
  .login-container {
    padding: 30px 25px;
    margin: 10px;
  }

  .login-title {
    font-size: 1.8rem;
  }

  .login-input {
    height: 52px;
  }
}
</style>