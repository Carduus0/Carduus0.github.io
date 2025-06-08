<template>
  <section id="education">
    <h3 class="section-title" @click="toggleCollapse">
      {{ $t('education.title') }}
      <span class="collapse-icon" v-if="isMobile && !isCollapsed">+</span>
      <span class="collapse-icon" v-if="isMobile && isCollapsed">-</span>
    </h3>
    <div class="section-content" :class="{ collapsed: isCollapsed && isMobile }">
      <ul class="left-aligned-list">
        <li>
          <a href="https://rs.school/courses" target="_blank"> RS_School Frontend </a>
        </li>
        <li>
          <a href="https://www.udemy.com" target="_blank">Udemy</a>
        </li>
        <li>
          <a
            href="https://ru.hexlet.io/courses/introduction_to_programming/lessons/types/exercise_unit"
            target="_blank"
          >
            Hexlet</a
          >
        </li>
        <li><a href="http://smolgmu.ru" target="_blank">SGMU</a></li>
      </ul>
    </div>
  </section>
</template>

<script setup lang="ts">
import { ref, onMounted, onUnmounted } from 'vue'

const isCollapsed = ref(false)
const isMobile = ref(false) // Состояние для отслеживания мобильного размера

const checkMobile = () => {
  isMobile.value = window.innerWidth <= 768 // Проверять на ваш брейкпоинт
  // Если на десктопе, всегда развернуто
  if (!isMobile.value) {
    isCollapsed.value = false
  }
}

const toggleCollapse = () => {
  if (isMobile.value) {
    // Переключаем только на мобильных
    isCollapsed.value = !isCollapsed.value
  }
}

onMounted(() => {
  checkMobile() // Проверяем при монтировании
  window.addEventListener('resize', checkMobile) // Слушаем изменение размера окна
})

onUnmounted(() => {
  window.removeEventListener('resize', checkMobile)
})
</script>

<style scoped>
/* Ваши существующие стили */
.section-title {
  cursor: pointer; /* Делаем заголовок кликабельным */
  display: flex;
  justify-content: space-between; /* Распределяем элементы по ширине */
  align-items: center;
}

.collapse-icon {
  font-size: 1.5em; /* Размер иконки */
  transition: transform 0.3s ease;
}

.section-content {
  overflow: hidden;
  max-height: fit-content;
  transition: max-height 0.3s ease-out;
}

.section-content.collapsed {
  max-height: 0;
}

@media (max-width: 768px) {
  .section-content {
    max-height: 0; /* По умолчанию свернуто на мобильных */
  }

  .section-content:not(.collapsed) {
    max-height: fit-content; /* Разворачиваем при активном состоянии */
  }

  .section-title .collapse-icon {
    transform: rotate(0deg); /* Иконка для развернутого состояния */
  }

  .section-title .collapse-icon.collapsed {
    transform: rotate(90deg); /* Иконка для свернутого состояния */
  }
}
</style>
