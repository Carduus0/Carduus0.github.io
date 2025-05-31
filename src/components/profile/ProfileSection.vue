<template>
  <div class="profile" id="Profile">
    <img src="/img/my_photo.jpg" width="100" class="avatar" alt="My_photo" />
    <h1 class="title">{{ $t('profile.title') }}</h1>
    <h2 class="dev">{{ $t('profile.developer') }}</h2>
  </div>
</template>

<script setup></script>

<style scoped>
.profile {
  display: flex;
  flex-direction: column;
  align-items: center;
  font-family: 'Roboto', sans-serif;
}

.avatar {
  border-radius: 10px;
  margin-top: 15px;
}

.title {
  font-size: 1.3em;
  margin-top: 0.5em;
}

.dev {
  display: inline-block;
  font-size: 1em;
}
</style>
