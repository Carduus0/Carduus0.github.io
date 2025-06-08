<template>
  <section id="languages">
    <h2 class="section-title" @click="toggleCollapse">
      {{ $t('languages.title') }}
      <span class="collapse-icon" v-if="isMobile" :class="{ rotated: !isCollapsed }">
        &#9660;
      </span>
    </h2>
    <div class="section-content" :class="{ collapsed: isCollapsed && isMobile }">
      <ul class="left-aligned-list">
        <li>{{ $t('languages.russian') }}</li>
        <li>{{ $t('languages.english') }}</li>
      </ul>
    </div>
  </section>
</template>

<script setup lang="ts">
import { ref, onMounted, onUnmounted } from 'vue'

const isCollapsed = ref(true) // Изначально свернуто на мобильных
const isMobile = ref(false)

const checkMobile = () => {
  isMobile.value = window.innerWidth <= 768
  if (!isMobile.value) {
    isCollapsed.value = false
  }
}

const toggleCollapse = () => {
  if (isMobile.value) {
    isCollapsed.value = !isCollapsed.value
  }
}

onMounted(() => {
  checkMobile()
  window.addEventListener('resize', checkMobile)
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
  transform: rotate(90deg);
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
