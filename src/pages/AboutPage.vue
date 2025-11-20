<template>
  <div class="about">
    <div class="about-header">
      <h1 class="about-title">О приложении</h1>
      <p class="about-subtitle">Зачем нужен ещё один To-Do?</p>
    </div>

    <transition name="fade" appear>
      <div v-if="visible" class="content">
        <div class="welcome-card">
          <p class="welcome-text">Добро пожаловать на информационную страницу моего To-Do :)</p>
          <p class="features-intro">Вот что вы можете делать:</p>
        </div>

        <transition-group name="list" tag="ul" class="features-list">
          <li
              v-for="(item, i) in features"
              :key="i"
              :style="{ '--delay': i }"
              class="feature-item"
          >
            <span class="feature-icon">✓</span>
            {{ item }}
          </li>
        </transition-group>
      </div>
    </transition>
  </div>

  <ThemeToggle />
</template>

<script setup>
import { ref, onMounted } from 'vue'
import ThemeToggle from '@/components/ThemeToggle.vue'

const visible = ref(false)

onMounted(() => {
  visible.value = true
})

const features = [
  'добавлять задачи и систематизировать свои дела',
  'удалять лишнее и поддерживать порядок',
  'отмечать выполнение и фиксировать прогресс',
  'переключать тему интерфейса под своё настроение'
]
</script>

<style scoped lang="scss">
@use '../scss/utils/variables' as *;
@use '../scss/utils/mixins' as *;

.about {
  width: 100%;
  max-width: 800px;
  padding: 40px;
  background: linear-gradient(135deg, var(--color-background-soft) 0%, var(--color-background) 100%);
  border-radius: 20px;
  margin: 60px auto;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.1);
  border: 1px solid var(--color-border);
  transition: all 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275);

  &:hover {
    transform: translateY(-5px);
    box-shadow: 0 20px 40px rgba(0, 0, 0, 0.15);
  }
}

.about-header {
  text-align: center;
  margin-bottom: 40px;
}

.about-title {
  font-size: 2.5rem;
  font-weight: 700;
  background: linear-gradient(135deg, var(--color-primary) 0%, var(--color-secondary) 100%);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
  margin-bottom: 10px;
}

.about-subtitle {
  font-size: 1.1rem;
  color: var(--color-text-soft);
  font-weight: 500;
}

.welcome-card {
  background: var(--color-background);
  padding: 25px;
  border-radius: 15px;
  margin-bottom: 30px;
  border: 1px solid var(--color-border);
  box-shadow: 0 4px 15px rgba(0, 0, 0, 0.05);
}

.welcome-text {
  font-size: 1.1rem;
  line-height: 1.6;
  color: var(--color-text);
  margin-bottom: 15px;
}

.features-intro {
  font-size: 1rem;
  color: var(--color-text);
  font-weight: 600;
}

.features-list {
  list-style: none;
  padding: 0;
  margin: 0;
}

.feature-item {
  display: flex;
  align-items: center;
  padding: 15px 20px;
  margin: 12px 0;
  background: var(--color-background);
  border-radius: 12px;
  border: 1px solid var(--color-border);
  font-size: 1rem;
  line-height: 1.5;
  color: var(--color-text);
  opacity: 0;
  animation: rise 0.6s forwards;
  animation-delay: calc(var(--delay) * 150ms);
  transition: all 0.3s ease;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.05);

  &:hover {
    transform: translateX(10px);
    border-color: var(--color-primary);
    box-shadow: 0 4px 15px rgba(0, 0, 0, 0.1);
  }
}

.feature-icon {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 24px;
  height: 24px;
  background: linear-gradient(135deg, var(--color-primary) 0%, var(--color-secondary) 100%);
  color: white;
  border-radius: 50%;
  margin-right: 15px;
  font-size: 0.8rem;
  font-weight: bold;
  flex-shrink: 0;
}

@keyframes rise {
  from {
    opacity: 0;
    transform: translateY(20px) scale(0.95);
  }
  to {
    opacity: 1;
    transform: translateY(0) scale(1);
  }
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
  transform: translateY(30px);
}

.fade-enter-active,
.fade-leave-active {
  transition: all 0.8s cubic-bezier(0.4, 0, 0.2, 1);
}

@media (max-width: 768px) {
  .about {
    margin: 30px 20px;
    padding: 30px 20px;
  }

  .about-title {
    font-size: 2rem;
  }

  .about-subtitle {
    font-size: 1rem;
  }

  .welcome-card {
    padding: 20px;
  }

  .feature-item {
    padding: 12px 15px;
    font-size: 0.95rem;
  }

  .feature-icon {
    width: 20px;
    height: 20px;
    margin-right: 12px;
    font-size: 0.7rem;
  }
}

@media (max-width: 480px) {
  .about {
    margin: 20px 15px;
    padding: 25px 15px;
  }

  .about-title {
    font-size: 1.8rem;
  }

  .feature-item:hover {
    transform: translateX(5px);
  }
}
</style>