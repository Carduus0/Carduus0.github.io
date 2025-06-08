<template>
  <section class="container__projects" id="projects">
    <h2 class="section-title">{{ $t('projects.title') }}</h2>
    <ul class="projects-grid">
      <li v-for="project in projects" :key="project.id" class="project-card">
        <div class="project-preview" @click="openProject(project)">
          <img
            :src="project.previewImage"
            :alt="$t(`projects.${project.id}.title`)"
            v-if="project.previewImage"
          />
          <div class="no-preview" v-else>{{ $t('projects.noPreview') }}</div>
          <h4 class="project-title">
            {{ $t(`projects.${project.id}.title`) }}
          </h4>
          <p class="project-description">
            {{ $t(`projects.${project.id}.description`) }}
          </p>
        </div>
        <div class="project-links">
          <a
            :href="project.link"
            target="_blank"
            rel="noopener noreferrer"
            v-if="project.link"
          >
            {{ $t('projects.view') }}
          </a>
          <button @click="openFullScreen(project)" v-if="project.fullScreenUrl">
            {{ $t('projects.fullScreen') }}
          </button>
        </div>

        <div
          v-if="selectedProject === project && isFullScreen"
          class="fullscreen-overlay"
          @click.self="closeFullScreen"
        >
          <button class="close-fullscreen" @click="closeFullScreen">×</button>
          <iframe
            v-if="project.fullScreenUrl"
            :src="project.fullScreenUrl"
            frameborder="0"
            allowfullscreen
          ></iframe>
          <div v-else class="no-fullscreen">
            {{ $t('projects.noFullScreenAvailable') }}
          </div>
        </div>
      </li>
    </ul>
  </section>
</template>

<script setup lang="ts">
import { ref } from 'vue'
import type { Project } from '../../types/project'

const isFullScreen = ref(false)
const selectedProject = ref<Project | null>(null)

const projects = ref<Project[]>([
  {
    id: 'provision_monitoring',
    title: 'Приложение Provision Monitoring',
    description:
      'Provision 360 — это интеллектуальная система мониторинга строительных объектов, разработанная компанией Mindset. Она сочетает технологии компьютерного зрения, 360°-съёмки и BIM-моделирования для автоматического контроля хода строительства и повышения прозрачности процессов.',
    link: 'https://provision360.ru/',
    previewImage: '/img/provision-preview.png',
    fullScreenUrl: null
  },
  {
    id: 'threejsTour1',
    title: 'Интерактивные 3D туры (Three.js).',
    description:
      '3D-туры Provision 360 — это виртуальные экскурсии по строительным объектам, совмещающие панорамные фото 360°, 3D-модели, полученные с помощью лазерного сканирования, и интерактивные инструменты управления.',
    link: null,
    previewImage: '/img/threejs-tour_example-preview.png',
    fullScreenUrl: 'https://link.provision360.ru/tour/zREAd8OD'
  },
  {
    id: 'text_summarizer',
    title: 'Text Summarizer',
    description:
      'Использует искусственный интеллект для краткого изложения текста, обеспечивая точные и актуальные обзоры. Расширение для браузеров.',
    link: 'https://chromewebstore.google.com/detail/%D0%BA%D1%80%D0%B0%D1%82%D0%BA%D0%B8%D0%B9-%D0%BF%D0%B5%D1%80%D0%B5%D1%81%D0%BA%D0%B0%D0%B7-%D1%81%D1%82%D1%80%D0%B0%D0%BD%D0%B8%D1%86%D1%8B/npifianbfjhobabjjpfdjjihgbdnbojh?hl=ru&authuser=0',
    previewImage: '/img/text-summarizer-preview.png',
    fullScreenUrl: null
  },
  {
    id: 'image_to_text',
    title: 'Image To Text',
    description:
      'Преобразование изображений в текст с помощью искусственного интеллекта. Расширение для браузеров.',
    link: 'https://chromewebstore.google.com/detail/image-to-text/jfpipjgidnagjbmdfhogcoklclacgnhk?hl=en&authuser=0',
    previewImage: '/img/image-to-text-preview.png',
    fullScreenUrl: null
  },
  {
    id: 'image_to_prompt',
    title: 'Image To Prompt',
    description:
      'Generate prompts from any picture with Image to Prompt! Расширение для браузеров.',
    link: 'https://chromewebstore.google.com/detail/image-to-prompt/opfppjjpcgojicomghpdjanjpeobaajo?hl=en&authuser=0',
    previewImage: '/img/image-to-prompt-preview.png',
    fullScreenUrl: null
  },
  {
    id: 'developmentButton',
    title: 'Development button',
    description:
      'Та самая кнопка, чтобы делать деньги, 300к/наносек) Автоматическое проигрывание трека запрещено.',
    link: 'https://carduus0.github.io/Fun-development-button/',
    previewImage: '/img/button-preview.png',
    fullScreenUrl: null
  },
  {
    id: 'shelter',
    title: 'Shelter',
    description: 'Лендинг для приюта домашних животных. Учебные проекты.',
    link: 'https://rolling-scopes-school.github.io/carduus0-JSFE2023Q1/shelter/#',
    previewImage: '/img/shelter-preview.png',
    fullScreenUrl: null
  },
  {
    id: 'plants',
    title: 'Plants',
    description: 'Лендинг о комнатных растениях. Учебные проекты.',
    link: 'https://rolling-scopes-school.github.io/carduus0-JSFEPRESCHOOL2022Q4/plants/',
    previewImage: '/img/plants-preview.png',
    fullScreenUrl: null
  }
  // {
  //   id: 'momentum',
  //   title: 'Momentum',
  //   description:
  //     'Клон популярного расширения с часами, погодой и цитатами. Учебные проекты.',
  //   link: 'https://rolling-scopes-school.github.io/carduus0-JSFEPRESCHOOL2022Q4/momentum/',
  //   previewImage: '/img/momentum-preview.png',
  //   fullScreenUrl: null
  // }
])

const openProject = (project: any) => {
  if (project.link) {
    window.open(project.link, '_blank')
  } else if (project.fullScreenUrl) {
    openFullScreen(project)
  }
}

const openFullScreen = (project: any) => {
  selectedProject.value = project
  isFullScreen.value = true
  // Отключение прокрутки фона при открытии модального окна
  document.body.style.overflow = 'hidden'
}

const closeFullScreen = () => {
  isFullScreen.value = false
  selectedProject.value = null
  // Включение прокрутки фона при закрытии модального окна
  document.body.style.overflow = ''
}
</script>

<style scoped>
.container__projects {
  padding-top: 20px;
}

.section-title {
  font-family: 'Roboto', sans-serif;
  margin-bottom: 1rem;
}

.projects-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  gap: 1.5rem;
  list-style: none;
  padding: 0;
}

.project-card {
  border: 1px solid #ddd;
  border-radius: 8px;
  overflow: hidden;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  display: flex;
  flex-direction: column;
}

.project-preview {
  cursor: pointer;
}

.project-preview img {
  width: 100%;
  height: auto;
  display: block;
}

.no-preview {
  background-color: #f0f0f0;
  color: #777;
  text-align: center;
  padding: 2rem;
}

.project-title {
  font-family: 'Roboto', sans-serif;
  padding: 1rem;
  margin: 0;
  font-size: 1.2rem;
  font-weight: bold;
}

.project-description {
  font-family: 'Roboto', sans-serif;
  padding: 0 1rem 1rem;
  margin: 0;
  color: #555;
  text-align: start;
}

.project-links {
  display: flex;
  padding: 1rem;
  gap: 0.5rem;
  margin-top: auto;
}

.project-links a,
.project-links button {
  flex-grow: 1;
  padding: 0.3rem 1rem;
  border: 1px solid #c37d4a;
  border-radius: 4px;
  font-family: 'Roboto', sans-serif;
  color: #c37d4a;
  background-color: transparent;
  text-align: center;
  text-decoration: none;
  cursor: pointer;
  transition: background-color 0.3s ease, box-shadow 0.2s ease-in-out,
    transform 0.1s ease-in-out;
  box-shadow: 1px 1px 3px rgba(0, 0, 0, 0.1);
}

.project-links a:hover,
.project-links button:hover {
  background-color: #f0f0f0;
}
.project-links a:active,
.project-links button:active {
  box-shadow: inset 0 2px 4px rgba(0, 0, 0, 0.2);
  transform: translateY(1px);
}

/* Fullscreen Overlay */
.fullscreen-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.9);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 1000;
}

.close-fullscreen {
  position: absolute;
  top: 1rem;
  right: 1rem;
  font-size: 2rem;
  color: white;
  background: none;
  border: none;
  cursor: pointer;
  z-index: 1001;
}

.fullscreen-overlay iframe {
  width: 90%;
  height: 90%;
  border: none;
}

.no-fullscreen {
  color: white;
  font-size: 1.5rem;
  text-align: center;
}
</style>
