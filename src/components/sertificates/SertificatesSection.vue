<template>
  <section class="container__sertificates" id="sertificates">
    <h2 class="section-title" @click="toggleCollapse">
      {{ $t('sertificates.title') }}
      <span class="collapse-icon" v-if="isMobile" :class="{ rotated: !isCollapsed }">
        &#9660;
      </span>
    </h2>
    <div class="section-content" :class="{ collapsed: isCollapsed && isMobile }">
      <img
        ref="sertificateImage1"
        src="/img/rs_school-0-min.png"
        alt="sertificate 1"
        @click.stop="toggleExpand1"
        :class="{ expanded: isExpanded1 }"
      />
      <button class="toggle-more-sertificates" @click="showMore = !showMore">
        {{ showMore ? $t('sertificates.less') : $t('sertificates.more') }}
      </button>
      <img
        v-if="showMore"
        ref="sertificateImage2"
        src="/img/gratitude-min.jpg"
        alt="sertificate 2"
        @click.stop="toggleExpand2"
        :class="{ expanded: isExpanded2 }"
      />
    </div>
  </section>
</template>

<script setup lang="ts">
import { ref, onMounted, onUnmounted } from 'vue'
import { useI18n } from 'vue-i18n'

const { t } = useI18n()
const isExpanded1 = ref(false)
const isExpanded2 = ref(false)
const showMore = ref(false)
const isCollapsed = ref(true) // Изначально свернуто на мобильных
const isMobile = ref(false)

const sertificateImage1 = ref<HTMLImageElement | null>(null)
const sertificateImage2 = ref<HTMLImageElement | null>(null)

const toggleExpand1 = () => {
  isExpanded1.value = !isExpanded1.value
}

const toggleExpand2 = () => {
  isExpanded2.value = !isExpanded2.value
}

const handleClickOutside = (event: MouseEvent) => {
  // Проверяем, если клик был не по первому изображению и не по второму изображению
  if (
    sertificateImage1.value &&
    !sertificateImage1.value.contains(event.target as Node) &&
    sertificateImage2.value &&
    !sertificateImage2.value.contains(event.target as Node)
  ) {
    isExpanded1.value = false
    isExpanded2.value = false
  }
}

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
    isCollapsed.value = !isCollapsed.value
    // При сворачивании секции, свернуть и все развернутые изображения
    if (isCollapsed.value) {
      isExpanded1.value = false
      isExpanded2.value = false
      showMore.value = false // Сворачиваем "ещё" тоже
    }
  }
}

onMounted(() => {
  checkMobile()
  window.addEventListener('resize', checkMobile)
  document.addEventListener('click', handleClickOutside)
})

onUnmounted(() => {
  window.removeEventListener('resize', checkMobile)
  document.removeEventListener('click', handleClickOutside)
})
</script>

<style scoped>
.container__sertificates {
  text-align: center;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.section-title {
  font-family: 'Roboto', sans-serif;
  display: flex;
  justify-content: space-between;
  align-items: center;
  width: 100%;
  margin-bottom: 5px;
}

.collapse-icon {
  font-size: 1.4rem;
  transition: transform 0.3s ease;
  cursor: pointer;
  transform: rotate(90deg);
  color: #333;
}

img {
  max-width: 140px;
  height: auto;
  cursor: pointer;
  transition: transform 0.3s ease-in-out, max-height 0.3s ease-in-out,
    max-width 0.3s ease-in-out;
  max-height: 200px;
  object-fit: contain;
}

.expanded {
  max-width: 100%;
  max-height: none;
  transform: scale(1);
  object-fit: contain;
}

.toggle-more-sertificates {
  padding: 0.2rem 0.5rem;
  border: 1px solid #c37d4a;
  border-radius: 4px;
  background: none;
  font-family: 'Roboto', sans-serif;
  color: #c37d4a;
  text-align: center;
  text-decoration: none;
  cursor: pointer;
  transition: background-color 0.3s ease, box-shadow 0.2s ease-in-out,
    transform 0.1s ease-in-out;
  box-shadow: 1px 1px 3px rgba(0, 0, 0, 0.1);
}

.toggle-more-sertificates:hover {
  background-color: #f0f0f0;
}
.toggle-more-sertificates:active {
  box-shadow: inset 0 2px 4px rgba(0, 0, 0, 0.2);
  transform: translateY(1px);
}

/* Стили для сворачивания секции */
.section-content {
  overflow: hidden;
  max-height: fit-content; /* По умолчанию развернуто на десктопе */
  transition: max-height 0.3s ease-out;
  width: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 10px;
}

.section-content.collapsed {
  max-height: 0;
}

@media (max-width: 768px) {
  .section-content {
    max-height: 0; /* На мобильных по умолчанию свернуто */
  }

  .section-content:not(.collapsed) {
    max-height: fit-content;
  }

  .section-title {
    cursor: pointer;
    padding-left: 10px;
  }

  .section-title .collapse-icon {
    transform: rotate(0deg);
  }

  .section-title .collapse-icon.rotated {
    transform: rotate(180deg);
  }
}
</style>
