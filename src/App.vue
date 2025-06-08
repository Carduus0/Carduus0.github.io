<template>
  <div id="page">
    <TheHeader />
    <main class="main" style="position: relative">
      <Mouse />
      <div class="main__two-column">
        <aside class="left">
          <ProfileSection />
          <SkillsSection />
          <EducationSection />
          <LanguagesSection />
          <SertificatesSection />
        </aside>

        <div class="right-side">
          <SummarySection />
          <ProjectsSection />
        </div>
      </div>
    </main>
    <TheFooter />
  </div>
</template>

<script setup>
import { TheHeader } from './components/header'
import { TheFooter } from './components/footer'
import { ProfileSection } from './components/profile'
import { SummarySection } from './components/summary'
import { SkillsSection } from './components/skills'
import { ProjectsSection } from './components/projects'
import { EducationSection } from './components/education'
import { LanguagesSection } from './components/languages'
import { SertificatesSection } from './components/sertificates'
import { Mouse } from './components/mouse'
</script>

<style scoped>
#page {
  font-family: 'Roboto', Verdana, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  width: 100%;
}

main {
  background-color: #f0f0f0;
  width: 100%;
  box-sizing: border-box;
}

.main__two-column {
  display: flex;
  max-width: 1230px;
  margin: 0 auto;
  box-sizing: border-box;
}

.left {
  max-width: 25%;
  display: flex;
  flex-direction: column;
  background-color: #f0f0f0;
  padding: 10px;
  box-shadow: 2px 0 5px rgba(0, 0, 0, 0.1);
}

ul {
  list-style-type: none;
  padding-left: 0;
}

.right-side {
  flex: 1; /* Занимает оставшееся пространство */
  padding: 10px;
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
    padding: 0 5px;
    box-shadow: none;
  }

  .left-top {
    order: 1;
  }

  .right-main {
    order: 2;
  }

  .left-bottom {
    order: 3;
  }

  .right-side {
    padding: 0 5px;
  }
}
</style>
