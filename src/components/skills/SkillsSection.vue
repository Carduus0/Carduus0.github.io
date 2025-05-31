<template>
  <section class="container container__skills" id="skills">
    <h2 class="section-title">{{ $t("skills.title") }}</h2>
    <div class="tag-cloud">
      <span v-for="skill in skills" :key="skill" class="skill-tag">{{
        skill
      }}</span>
    </div>
  </section>
</template>

<script setup>
import { ref } from "vue";

const skills = ref([
  "JavaScript",
  "TypeScript",
  "Vue.js",
  "Vue 3",
  "Vue 2",
  "Composition API",
  "Pinia",
  "Vue Router",
  "Vuetify",
  "NaiveUI",
  "Quasar",
  "HTML5",
  "SCSS",
  "CSS Grid",
  "Flexbox",
  "BEM",
  "FSD",
  "REST API",
  "WebSocket",
  "Axios",
  "fetch",
  "GraphQL",
  "OAuth2.0",
  "i18n",
  "Three.js",
  "WebGL",
  "Chrome Extensions",
  "Vite",
  "Webpack",
  "npm",
  "Docker",
  "Git",
  "Strapi",
  "Node.js",
  "Express",
  "Capacitor",
  "SPA",
  "Figma",
  "Pixel Perfect",
  "ES6",
  "Medical Degree",
]);
</script>

<style scoped>
@import "@/styles/global.css";
.container__skills {
  padding-bottom: 20px; /* Добавим немного отступа снизу секции */
}

.section-title {
  font-family: "Roboto", sans-serif;
  padding-top: 20px;
  margin-bottom: 15px; /* Отступ перед облаком тегов */
}

.tag-cloud {
  display: flex;
  flex-wrap: wrap;
  gap: 8px; /* Расстояние между тегами */
}

.skill-tag {
  display: inline-block;
  padding: 8px 12px;
  background-color: #f0f0f0;
  font-family: "Roboto", sans-serif;
  color: #333;
  border-radius: 5px;
  font-size: 14px;
  transition: background-color 0.3s ease;
}

.skill-tag:hover {
  background-color: #ddd;
}
</style>
