<template>
  <footer class="footer container">
    <div class="footer__wrapper">
      <p>©2025</p>
      <a href="https://github.com/Carduus0"> Carduus0</a>
    </div>
  </footer>
</template>

<script setup></script>

<style scoped>
.footer {
  background-color: #c37d4a;
  padding: 10px 15px; /* Добавим отступы */
}

.footer__wrapper {
  display: flex;
  justify-content: space-around;
  align-items: center;
  padding: 10px 0;
}

.footer__wrapper a {
  font-family: 'Roboto', sans-serif;
  color: #fff;
  text-decoration: none;
}

.footer__wrapper a:hover {
  color: orange;
}
</style>
