<template>
  <section class="container__skills" id="skills">
    <h2 class="section-title skills-title" @click="toggleCollapse">
      {{ $t('skills.title') }}
      <span class="collapse-icon" v-if="isMobile" :class="{ rotated: !isCollapsed }">
        &#9660;
      </span>
    </h2>
    <div
      class="tag-cloud section-content"
      :class="{ collapsed: isCollapsed && isMobile }"
    >
      <span v-for="skill in skills" :key="skill" class="skill-tag">{{ skill }}</span>
    </div>
  </section>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

const skills = ref([
  'JavaScript',
  'TypeScript',
  'Vue.js',
  'Composition API',
  'Pinia',
  'Vue Router',
  'Vuetify',
  'NaiveUI',
  'Quasar',
  'HTML5',
  'SCSS',
  'CSS Grid',
  'Flexbox',
  'BEM',
  'FSD',
  'REST API',
  'WebSocket',
  'GraphQL',
  'OAuth2.0',
  'i18n',
  'Three.js',
  'WebGL',
  'Chrome Extensions',
  'Vite',
  'Webpack',
  'npm',
  'Docker',
  'Git',
  'Strapi',
  'Node.js',
  'Express',
  'Capacitor',
  'SPA',
  'Figma',
  'Pixel Perfect',
  'ES6',
  'Medical Degree'
])

const isCollapsed = ref(true)
const isMobile = ref(false)

const checkMobile = () => {
  isMobile.value = window.innerWidth <= 768
  if (!isMobile.value) {
    isCollapsed.value = false
  } else {
    isCollapsed.value = true
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
.skills-title {
  cursor: pointer;
  display: flex;
  justify-content: space-between;
  align-items: center;
  width: 100%;
}

.tag-cloud {
  display: flex;
  flex-wrap: wrap;
  gap: 8px;
}

.skill-tag {
  display: inline-block;
  padding: 6px;
  background-color: #f0f0f0;
  font-family: 'Roboto', sans-serif;
  color: #333;
  border-radius: 5px;
  font-family: 'Roboto', sans-serif;
  font-size: 0.8em;
  transition: background-color 0.3s ease;
}

.skill-tag:hover {
  background-color: #ddd;
}

.collapse-icon {
  font-size: 1.4rem;
  transition: transform 0.3s ease;
  cursor: pointer;
  transform: rotate(90deg);
  color: #333;
}
.section-content {
  max-height: fit-content;
  padding: 0 5px;
  margin-top: 5px;
  transition: max-height 0.3s ease-out;
  width: 100%;
  display: flex;
  flex-direction: row;
  justify-content: center;
  flex-wrap: wrap;
  gap: 8px;
  overflow: hidden;
}

.section-content.collapsed {
  max-height: 0;
}

@media (max-width: 768px) {
  .skill-tag {
    font-size: 1em;
  }

  .skills-title {
    cursor: pointer;
    padding-left: 10px;
  }
  .section-content {
    max-height: 0;
    justify-content: center;
  }

  .section-content:not(.collapsed) {
    max-height: fit-content;
  }

  .section-title .collapse-icon {
    transform: rotate(0deg);
  }

  .section-title .collapse-icon.rotated {
    transform: rotate(180deg);
  }
}
</style>
