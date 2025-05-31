<template>
  <section class="container" id="education">
    <h3 class="section-title">{{ $t("education.title") }}</h3>
    <ul class="left-aligned-list">
      <li>
        <a href="https://rs.school/courses" target="_blank">
          RS_School Frontend
        </a>
      </li>
      <li>
        <a href="https://www.udemy.com" target="_blank">Udemy</a>
      </li>
      <li>
        <a
          href="https://ru.hexlet.io/courses/introduction_to_programming/lessons/types/exercise_unit"
          target="_blank"
        >
          Hexlet</a
        >
      </li>
      <li><a href="http://smolgmu.ru" target="_blank">SGMU</a></li>
    </ul>
  </section>
</template>

<script setup></script>

<style scoped>
@import "@/styles/global.css";
.section-title {
  font-family: "Roboto", sans-serif;
  padding-top: 20px;
}

a {
  font-family: "Roboto", sans-serif;
  text-decoration: none;
  color: black;
}

a:hover {
  color: orange;
}
</style>
