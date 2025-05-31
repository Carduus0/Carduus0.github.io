<template>
  <section class="container container__summary" id="summary">
    <h3 class="section-title">{{ $t("summary.title") }}</h3>
    <p>{{ $t("summary.paragraph1") }}</p>
    <p>{{ $t("summary.paragraph2") }}</p>
  </section>
</template>

<script setup></script>

<style scoped>
@import "@/styles/global.css";
.container__summary {
  /* Ваши стили для summary */
}

.section-title {
  font-family: "Roboto", sans-serif;
  padding-top: 20px;
}

p {
  font-family: "Roboto", sans-serif;
  margin-left: 5%;
}
</style>
