<template>
  <section>
    <div class="profile" id="Profile">
      <img src="/img/my_photo.jpg" width="100" class="avatar" alt="My_photo" />
      <h2 class="title">{{ $t('profile.title') }}</h2>
      <h1 class="dev">{{ $t('profile.developer') }}</h1>
      <!-- <ContactSection /> -->
      <div class="contacts" id="contacts">
        <ul class="social-icons">
          <li>
            <img src="/img/telegram_ico32.png" alt="Telegram_icon" />
            <a href="https://t.me/JS_SvetKhan">AlsoTelegram</a>
          </li>
          <li>
            <img src="/img/icon_Email.png" alt="Email_icon" />
            <a href="mailto:s.carduus@gmail.com">s.carduus@gmail.com</a>
          </li>
          <li>
            <img src="/img/discord_logo32.png" alt="Discord_icon" />
            <a href="#"> Carduus0</a>
          </li>
        </ul>
      </div>
    </div>
  </section>
</template>

<script setup>
import { ContactSection } from '../../components/contact'
</script>

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
  font-size: 1em;
  margin-top: 0.5em;
  margin-bottom: 0.3em;
}

.dev {
  display: inline-block;
  font-size: 1em;
}

.contacts {
  width: 100%;
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
  width: 24px;
  height: 24px;
}

.social-icons a {
  text-decoration: none;
  color: #333;
  font-family: 'Roboto', sans-serif;
  font-size: 1em;
}

.social-icons a:hover {
  color: #c37d4a;
}
@media (max-width: 768px) {
  .title,
  .dev {
    font-size: 1.5em;
  }

  .contacts {
    padding-left: 10px;
  }
  .social-icons a {
    font-size: 1.3em;
  }

  .social-icons img {
    width: 30px;
    height: 30px;
  }
  .social-icons li {
    margin-bottom: 12px;
  }
}
</style>
