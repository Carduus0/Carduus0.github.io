<template>
  <section class="container__sertificates" id="sertificates">
    <!-- <h3 class="section-title">{{ $t('sertificates.title') }}</h3> -->
    <img
      ref="gratitudeImage1"
      src="/img/gratitude-min.jpg"
      alt="gratitude 1"
      @click.stop="toggleExpand1"
      :class="{ expanded: isExpanded1 }"
    />
    <button class="toggle-more-sertificates" @click="showMore = !showMore">
      {{ showMore ? $t('sertificates.less') : $t('sertificates.more') }}
    </button>
    <img
      v-if="showMore"
      ref="gratitudeImage2"
      src="/img/rs_school-0-min.png"
      alt="gratitude 2"
      @click.stop="toggleExpand2"
      :class="{ expanded: isExpanded2 }"
    />
  </section>
</template>

<script setup lang="ts">
import { onMounted, onUnmounted, ref } from 'vue'
//import { useI18n } from 'vue-i18n'

//const { t } = useI18n
const isExpanded1 = ref(false)
const isExpanded2 = ref(false)
const showMore = ref(false)
const gratitudeImage1 = ref<HTMLImageElement | null>(null)
const gratitudeImage2 = ref<HTMLImageElement | null>(null)

const toggleExpand1 = () => {
  isExpanded1.value = !isExpanded1.value
}

const toggleExpand2 = () => {
  isExpanded2.value = !isExpanded2.value
}

const handleClickOutside = (event: MouseEvent) => {
  if (gratitudeImage1.value && !gratitudeImage1.value.contains(event.target as Node)) {
    isExpanded1.value = false
  }
  if (gratitudeImage2.value && !gratitudeImage2.value.contains(event.target as Node)) {
    isExpanded2.value = false
  }
}

onMounted(() => {
  document.addEventListener('click', handleClickOutside)
})

onUnmounted(() => {
  document.removeEventListener('click', handleClickOutside)
})
</script>

<style scoped>
.container__sertificates {
  padding-top: 20px;
  text-align: center;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 10px;
}

.section-title {
  font-family: 'Roboto', sans-serif;
  margin-bottom: 0.5em;
}

img {
  max-width: 140px;
  height: auto;
  cursor: pointer;
  transition: transform 0.3s ease-in-out, max-height 0.3s ease-in-out;
  max-height: 200px;
  object-fit: contain;
}

.expanded {
  max-width: 100%;
  max-height: none;
  transform: scale(1.05);
  object-fit: initial;
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
</style>
