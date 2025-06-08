<template>
  <section id="education">
    <h2 class="section-title" @click="toggleCollapse">
      {{ $t('education.title') }}
      <span class="collapse-icon" v-if="isMobile" :class="{ rotated: !isCollapsed }">
        &#9660;
      </span>
    </h2>
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

const isCollapsed = ref(false) // Изначально свернуто на мобильных
const isMobile = ref(false) // Состояние для отслеживания мобильного размера

const checkMobile = () => {
  isMobile.value = window.innerWidth <= 768
  if (!isMobile.value) {
    isCollapsed.value = false // На десктопе всегда развернуто
  } else {
    isCollapsed.value = true // На мобильных по умолчанию свернуто
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
.section-title {
  font-family: 'Roboto', sans-serif;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.collapse-icon {
  font-size: 1.4rem;
  transition: transform 0.3s ease;
  cursor: pointer;
  transform: rotate(0deg);
  color: #333;
}

/* Стили для сворачивания секции */
.section-content {
  overflow: hidden;
  max-height: fit-content; /* По умолчанию развернуто на десктопе */
  transition: max-height 0.3s ease-out;
  width: 100%;
  display: flex;
  flex-direction: column;
  align-items: start;
  gap: 10px;
}

.section-content.collapsed {
  max-height: 0;
}

@media (max-width: 768px) {
  .section-title {
    padding-left: 10px;
  }
  .section-content {
    max-height: 0; /* На мобильных по умолчанию свернуто */
    padding-left: 10px;
  }

  .section-content:not(.collapsed) {
    max-height: fit-content;
  }

  .section-title {
    cursor: pointer;
  }

  .section-title .collapse-icon {
    transform: rotate(0deg);
  }

  .section-title .collapse-icon.rotated {
    transform: rotate(180deg);
  }
}
</style>
