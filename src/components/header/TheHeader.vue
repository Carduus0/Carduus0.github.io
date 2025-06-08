<template>
  <header class="header__wrapper">
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
          <a href="#education" class="nav-link" @click="closeMobileNav">{{
            t('header.education')
          }}</a>
        </li>

        <li class="nav-item">
          <a href="#languages" class="nav-link" @click="closeMobileNav">{{
            t('header.languages')
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
          <a href="#projects" class="nav-link" @click="closeMobileNav">{{
            t('header.projects')
          }}</a>
        </li>
      </ul>
    </nav>
    <div class="language-switcher">
      <button :class="{ active: currentLocale === 'en' }" @click="setLocale('en')">
        EN
      </button>
      <button :class="{ active: currentLocale === 'ru' }" @click="setLocale('ru')">
        RU
      </button>
    </div>
  </header>
</template>

<script setup lang="ts">
import { useI18n } from 'vue-i18n'
import { onMounted, ref } from 'vue'

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
  padding: 5px 15px;
}

.burger-menu {
  display: none;
  cursor: pointer;
  padding: 10px;
}

.burger {
  position: relative;
  width: 30px;
  height: 22px;
}

.burger span {
  display: block;
  position: absolute;
  width: 100%;
  height: 2px;
  background-color: #f9f9f9;
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
  top: 50%;
  transform: translateY(-1px) rotate(45deg);
}

.burger__open .burger__line2 {
  opacity: 0;
}

.burger__open .burger__line3 {
  top: 50%;
  transform: translateY(-1px) rotate(-45deg);
}

.mobile-nav-open .nav-list {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 10px 0;
  justify-items: start;

  position: fixed;
  top: 42px;
  left: 5px;
  width: calc(100% - 11px);
  background-color: #c37d4a;
  border: none;
  padding: 6px;
  z-index: 10;
}

.mobile-nav-open .nav-item {
  margin-left: 0;
  margin-bottom: 0;
}

.mobile-nav-open .nav-link {
  color: white;
  padding: 0.5rem 0;
  line-height: 1.5;
  font-size: 1.2em;
  font-weight: bold;
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
  line-height: 40px;
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
  padding: 0.2rem;
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
    position: sticky;
    top: 0;
    justify-content: space-between;
    background-color: rgba(195, 125, 74, 0.9);
    padding: 0 6px;
    z-index: 100;
  }
  .nav-list {
    display: none;
  }

  .burger-menu {
    display: block;
  }

  .language-switcher button {
    justify-content: flex-start;
    gap: 0.5rem;
    font-size: 1.1rem;
  }
}
</style>
