<template>
  <section class="container__summary" id="summary">
    <p class="paragraph">{{ $t('summary.paragraph1') }}</p>
  </section>
</template>

<script setup></script>

<style scoped>
.section-title {
  font-family: 'Roboto', sans-serif;
  padding-top: 20px;
}

.paragraph {
  font-family: 'Roboto', sans-serif;
  font-size: 1em;
  text-align: start;
}

@media (max-width: 768px) {
  .paragraph {
    font-size: 1.17em;
  }
}
</style>
