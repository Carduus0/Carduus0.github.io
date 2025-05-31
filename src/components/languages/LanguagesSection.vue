<template>
  <section class="container container__languages" id="languages">
    <h3 class="section-title">{{ $t("languages.title") }}</h3>
    <ul class="left-aligned-list">
      <li>{{ $t("languages.russian") }}</li>
      <li>{{ $t("languages.english") }}</li>
    </ul>
  </section>
</template>

<script setup></script>

<style scoped>
@import "@/styles/global.css";
.container__languages {
  margin-bottom: 20px;
}

.section-title {
  font-family: "Roboto", sans-serif;
  padding-top: 20px;
}

ul {
  font-family: "Roboto", sans-serif;
  list-style: none;
}
</style>
