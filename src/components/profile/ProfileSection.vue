<template>
  <aside class="left">
    <div class="profile" id="Profile">
      <img src="/img/avatar.bmp" width="100" class="avatar" alt="My_photo" />
      <h1 class="title">{{ $t("profile.title") }}</h1>
      <h2 class="dev">{{ $t("profile.developer") }}</h2>
    </div>
  </aside>
</template>

<script setup></script>

<style scoped>
@import "@/styles/global.css";
.left {
  max-width: 35%;
  margin: 0 auto;
}

.profile {
  display: flex;
  flex-direction: column;
  align-items: center;
  font-family: "Roboto", sans-serif;
}

.avatar {
  border-radius: 10px;
  margin-top: 15px;
}

.title {
  font-size: 1.8em;
}

.dev {
  display: inline-block;
}
</style>
