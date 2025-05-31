<template>
  <header class="header__wrapper container">
    <div
      class="burger-menu"
      @click="toggleMobileNav"
      :class="{ burger__open: isMobileNavOpen }"
    >
      <div class="burger burger">
        <span class="burger__line1"></span>
        <span class="burger__line2"></span>
        <span class="burger__line3"></span>
      </div>
    </div>
    <nav class="nav" :class="{ 'mobile-nav-open': isMobileNavOpen }">
      <ul class="nav-list">
        <li class="nav-item">
          <a href="#contacts" class="nav-link" @click="closeMobileNav">{{
            t('header.contacts')
          }}</a>
        </li>
        <li class="nav-item">
          <a href="#summary" class="nav-link" @click="closeMobileNav">{{
            t('header.summary')
          }}</a>
        </li>
        <li class="nav-item">
          <a href="#skills" class="nav-link" @click="closeMobileNav">{{
            t('header.skills')
          }}</a>
        </li>
        <li class="nav-item">
          <a href="#my-git" class="nav-link" @click="closeMobileNav">{{
            t('header.code')
          }}</a>
        </li>
        <li class="nav-item">
          <a href="#education" class="nav-link" @click="closeMobileNav">{{
            t('header.education')
          }}</a>
        </li>
        <li class="nav-item">
          <a href="#projects" class="nav-link" @click="closeMobileNav">{{
            t('header.projects')
          }}</a>
        </li>
        <li class="nav-item">
          <a href="#languages" class="nav-link" @click="closeMobileNav">{{
            t('header.languages')
          }}</a>
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
import { onMounted, ref, nextTick } from 'vue'

const { t, locale } = useI18n()
const currentLocale = ref(localStorage.getItem('locale') || 'ru')
const isMobileNavOpen = ref(false)

function setLocale(newLocale: string) {
  currentLocale.value = newLocale
  locale.value = newLocale
  localStorage.setItem('locale', newLocale)
}

function toggleMobileNav() {
  isMobileNavOpen.value = !isMobileNavOpen.value
}
function closeMobileNav() {
  isMobileNavOpen.value = false
}

onMounted(() => {
  if (locale.value !== currentLocale.value) {
    locale.value = currentLocale.value
  }
})
</script>

<style scoped>
.header__wrapper {
  position: relative;
  display: flex;
  flex-wrap: wrap;
  justify-content: space-around;
  align-items: center;
  background-color: #c37d4a;
  padding: 5px 15px; /* Добавим отступы */
}

.burger-menu {
  display: none; /* Скрываем на десктопе */
  cursor: pointer;
  padding: 10px;
}

.burger {
  position: relative;
  width: 30px;
  height: 22px; /* Чтобы вместить все линии */
}

.burger span {
  display: block;
  position: absolute;
  width: 100%;
  height: 2px;
  background-color: black; /* Или ваш цвет */
  transition: transform 0.3s ease-in-out;
}

.burger__line1 {
  top: 0;
}

.burger__line2 {
  top: 50%;
  transform: translateY(-50%);
}

.burger__line3 {
  bottom: 0;
}

.burger__open .burger__line1 {
  top: 50%; /* Позиционируем по центру вертикали */
  transform: translateY(-1px) rotate(45deg);
}

.burger__open .burger__line2 {
  opacity: 0;
}

.burger__open .burger__line3 {
  top: 50%; /* Позиционируем по центру вертикали */
  transform: translateY(-1px) rotate(-45deg);
}

.mobile-nav-open .nav-list {
  display: flex !important;
  display: inline-block;
  flex-direction: column;
  align-items: flex-start;
  position: fixed;
  top: 42px;
  left: 5px;
  width: calc(100% - 11px);
  background-color: #c37d4a;
  border: none;
  padding: 6px;
  z-index: 10; /* Чтобы быть над контентом */
}

.mobile-nav-open .nav-item {
  margin-left: 0;
  margin-bottom: 1rem;
}

.mobile-nav-open .nav-link {
  color: white;
  padding: 0.5rem 0;
  line-height: 1.5;
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

@media (max-width: 768px) {
  .header__wrapper {
    position: sticky; /* Приклеиваем хедер */
    top: 0;
    justify-content: flex-start;
    background-color: rgba(195, 125, 74, 0.9);
    padding: 0 6px;
    z-index: 100;
  }
  .nav-list {
    display: none; /* Скрываем основное меню */
  }

  .burger-menu {
    display: block;
  }

  .language-switcher {
    margin-top: 1rem;
    justify-content: flex-start;
    gap: 0.5rem;
  }
}
</style>
