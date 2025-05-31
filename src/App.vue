<template>
  <div id="app">
    <TheHeader />
    <main class="main" style="position: relative">
      <Mouse />
      <div class="main__two-column container">
        <aside class="left">
          <ProfileSection />
          <ContactSection />
          <EducationSection />
          <LanguagesSection />
        </aside>

        <div class="right-side">
          <SummarySection />
          <SkillsSection />
          <ProjectsSection />
        </div>
      </div>
    </main>
    <TheFooter />
  </div>
</template>

<script setup>
import { TheHeader } from "./components/header";
import { TheFooter } from "./components/footer";
import { ProfileSection } from "./components/profile";
import { ContactSection } from "./components/contact";
import { SummarySection } from "./components/summary";
import { SkillsSection } from "./components/skills";
import { ProjectsSection } from "./components/projects";
import { EducationSection } from "./components/education";
import { LanguagesSection } from "./components/languages";
import { Mouse } from "./components/mouse";
</script>

<style scoped>
*,
*::before,
*::after {
  box-sizing: border-box;
}
#app {
  font-family: "Roboto", Verdana, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  width: 100%;
}

main {
  background-color: #f6f7e7;
  padding: 15px;
  width: 100%;
  box-sizing: border-box;
}

.main__two-column {
  display: flex;
  max-width: 1230px;
  margin: 0 auto;
  padding: 0 15px;
  box-sizing: border-box;
}

.left {
  max-width: 35%;
  display: flex;
  flex-direction: column;
}

ul {
  list-style-type: none;
  padding-left: 0;
}

.right-side {
  flex: 1; /* Занимает оставшееся пространство */
  padding: 0 10px;
}

@media (max-width: 768px) {
  .main__two-column {
    flex-direction: column;
    padding: 0 6px;
  }

  .left {
    max-width: 100%;
    margin-right: 0;
    margin-bottom: 20px;
    padding: 0 10px;
  }

  .right-side {
    width: 100%;
    padding: 0 10px;
  }

  main > div > div > section {
    /* Выберите нужные селекторы */
    padding-left: 10px;
    padding-right: 10px;
  }
}
</style>
