<template>
  <section class="my-git" id="my-git">
    <h3>{{ $t('myGit.title') }}</h3>
    <a href="https://github.com" target="_blank">{{ $t('myGit.title') }}</a>
  </section>
</template>

<script setup></script>

<style scoped>
.my-git {
  margin-bottom: 20px;
}

h3 {
  padding-top: 20px;
}

a {
  color: black;
  text-decoration: none;
}

a:hover {
  color: orange;
}
</style>
