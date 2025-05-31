<template>
  <header class="header__wrapper container">
    <nav class="nav">
      <ul class="nav-list">
        <li class="nav-item">
          <a href="#contacts" class="nav-link">{{ t('header.contacts') }}</a>
        </li>
        <li class="nav-item">
          <a href="#summary" class="nav-link">{{ t('header.summary') }}</a>
        </li>
        <li class="nav-item">
          <a href="#skills" class="nav-link">{{ t('header.skills') }}</a>
        </li>
        <li class="nav-item">
          <a href="#my-git" class="nav-link">{{ t('header.code') }}</a>
        </li>
        <li class="nav-item">
          <a href="#education" class="nav-link">{{ t('header.education') }}</a>
        </li>
        <li class="nav-item">
          <a href="#projects" class="nav-link">{{ t('header.projects') }}</a>
        </li>
        <li class="nav-item">
          <a href="#languages" class="nav-link">{{ t('header.languages') }}</a>
        </li>
        <li class="nav-item language-switcher">
          <button :class="{ active: currentLocale === 'en' }" @click="setLocale('en')">
            EN
          </button>
          <button :class="{ active: currentLocale === 'ru' }" @click="setLocale('ru')">
            RU
          </button>
        </li>
      </ul>
    </nav>
  </header>
</template>

<script setup lang="ts">
import { useI18n } from 'vue-i18n'
import { onMounted, ref } from 'vue'

const { t, locale } = useI18n()
const currentLocale = ref(localStorage.getItem('locale') || 'ru')

function setLocale(newLocale: string) {
  currentLocale.value = newLocale
  locale.value = newLocale
  localStorage.setItem('locale', newLocale)
}

onMounted(() => {
  if (locale.value !== currentLocale.value) {
    locale.value = currentLocale.value
  }
})
</script>

<style scoped>
@import '@/styles/global.css';

.header__wrapper {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-around;
  align-items: center;
  background-color: #c37d4a;
  padding: 10px 15px; /* Добавим отступы */
}

.nav-list {
  display: flex;
  justify-content: flex-end;
  align-items: center;
  flex-wrap: wrap;
  list-style: none;
  margin: 0;
  padding: 0;
}

.nav-item {
  margin-left: 20px;
  font-family: 'Roboto', sans-serif;
}

.nav-link {
  padding: 0 10px;
  line-height: 40px; /* Уменьшим высоту линии */
  text-decoration: none;
  color: black;
}

.nav-link:hover {
  color: orange;
}

.language-switcher {
  display: flex;
  gap: 0.2rem;
}

.language-switcher button {
  padding: 0.2rem 0.4rem;
  border: 1px solid transparent;
  cursor: pointer;
  background-color: transparent;
  font-size: 0.8rem;
  border-radius: 5px;
}

.language-switcher button:hover {
  background-color: rgba(0, 0, 0, 0.1);
  outline: none;
  border: 1px solid #c37d4a;
}

.language-switcher button.active {
  background-color: #f0f0f074;
}

.language-switcher button:focus-visible {
  outline: 2px solid #c37d4a;
}
</style>
