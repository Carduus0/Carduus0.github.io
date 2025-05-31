<template>
  <section class="container section__contacts" id="Contacts">
    <h2 class="section-title">{{ $t("header.contacts") }}</h2>
    <ul class="social-icons">
      <li>
        <img src="/img/icon_Email.png" alt="Email_icon" />
        <a href="mailto:s.carduus@gmail.com">s.carduus@gmail.com</a>
      </li>
      <li>
        <img src="/img/telegram_ico32.png" alt="Telegram_icon" />
        <a href="https://telegram.me/AlsoTelegram">AlsoTelegram</a>
      </li>
      <li>
        <img src="/img/discord_logo32.png" alt="Discord_icon" />
        <a href="#"> Carduus0</a>
      </li>
    </ul>
  </section>
</template>

<script setup></script>

<style scoped>
@import "@/styles/global.css";
.section__contacts {
  /* Ваши стили для контактов */
}

.section-title {
  font-family: "Roboto", sans-serif;
  padding-top: 20px;
}

.social-icons {
  list-style: none;
  padding-left: 0;
}

.social-icons li {
  display: flex;
  align-items: center;
  margin-bottom: 10px;
}

.social-icons img {
  margin-right: 10px;
  width: 32px; /* Примерный размер иконок */
  height: 32px;
}

.social-icons a {
  text-decoration: none;
  color: black;
}

.social-icons a:hover {
  color: orange;
}
</style>
