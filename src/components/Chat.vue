<template>
  <section class="chat">
    <div class="chat-container chat-bg">
      <h1 class="visually-hidden">Chat</h1>
      <div class="chat__manager chat-manager">
        <div class="chat-manager__avatar">
          <img
            class="chat-manager__avatar-img"
            src="../assets/avatar-img.jpg"
            alt="avatar"
          />
        </div>
        <div class="chat-manager__text">
          <h3 class="chat-manager__name text-big">Вероника Ростова</h3>
          <span class="chat-manager__position"> Менеджер по продажам </span>
          <blockquote class="chat-manager__quote">
            <p class="chat-manager__quote-paragraph">
              Подберу для вас самые лучшие предложения. Мои услуги абсолютно
              бесплатны
            </p>
          </blockquote>
        </div>
      </div>
      <div class="chat__services chat-services">
        <span class="chat-services__service">Услуг</span>
        <div class="chat-services__inner">
          <ul class="chat-services__items">
            <li class="chat-services__item services-item">
              <span class="services-item-name"> Ручное бронирование </span>
              <span class="services-item-amount"> 11 </span>
            </li>
            <li class="chat-services__item services-item">
              <span class="services-item-name"> Пакетные туры </span>
              <span class="services-item-amount"> 3 </span>
            </li>
            <li class="chat-services__item services-item">
              <span class="services-item-name"> Отели </span>
              <span class="services-item-amount"> 1 </span>
            </li>
          </ul>
        </div>
        <div class="chat-services__total">
          <span class="chat-services__total-text text-big">Всего</span>
          <span class="chat-services__total-num text-big">15</span>
        </div>
      </div>
      <div class="chat__feedback chat-feedback">
        <span class="chat-feedback__last text-big">Последние отзывы</span>
        <a class="chat-feedback__link" href="/">Все отзывы</a>
        <ul class="chat-feedback__icons">
          <li class="chat-feedback__icon chat-feedback__icon-like">131</li>
          <li class="chat-feedback__icon chat-feedback__icon-comment">14</li>
        </ul>
      </div>
      <div class="chat__messages chat-messages">
        <article class="chat-messages__item">
          <header class="chat-messages__header">
            <h3 class="chat-messages__user-name">
              Самуил<time datetime="2011-10-13" class="chat-messages__date"
                >13 октября 2011</time
              >
            </h3>
          </header>
          <p class="chat-messages__message">
            Привет, Верунь! ниче себе ты крутая. фотка класс!!!!
          </p>
        </article>
        <article class="chat-messages__item">
          <header class="chat-messages__header">
            <h3 class="chat-messages__user-name">
              Лилия Семёнова<time
                datetime="2011-10-14"
                class="chat-messages__date"
                >14 октября 2011</time
              >
            </h3>
          </header>
          <p class="chat-messages__message">
            Вероника, здравствуйте! Есть такой вопрос: Особый вид куниц жизненно
            стабилизирует кинетический момент, это и есть всемирно известный
            центр огранки алмазов и торговли бриллиантами?
          </p>
        </article>
        <article class="chat-messages__item">
          <header class="chat-messages__header">
            <h3 class="chat-messages__user-name">
              Лилия Семёнова<time
                datetime="2011-10-14"
                class="chat-messages__date"
                >14 октября 2011</time
              >
            </h3>
          </header>
          <p class="chat-messages__message">
            Вероника, здравствуйте! Есть такой вопрос: Особый вид куниц жизненно
            стабилизирует кинетический момент?
          </p>
        </article>
        <article
          v-for="(message, index) of messages"
          :key="index"
          class="chat-messages__item"
        >
          <header class="chat-messages__header">
            <h3 class="chat-messages__user-name">
              {{ message.name }}
              <time datetime="2011-10-14" class="chat-messages__date">
                {{ message.date }}
              </time>
            </h3>
          </header>
          <p class="chat-messages__message">
            {{ message.message }}
          </p>
        </article>
      </div>
    </div>
    <div class="chat-container chat-form-bg">
      <form @submit.prevent="onSubmit" class="chat__form">
        <textarea
          @keydown.ctrl.enter="onSubmit"
          v-model.trim="userMessage"
          required
          class="chat__form-text"
        ></textarea>
        <button class="chat__form-btn text-big" type="submit">
          Написать консультанту
        </button>
      </form>
    </div>
  </section>
</template>

<script>
export default {
  name: "Chat",
  data() {
    return {
      userName: "Аноним",
      userMessage: "",
      messages: [],
    };
  },
  created() {
    const messagesData = localStorage.getItem("chat-messages");
    if (messagesData) {
      this.messages = JSON.parse(messagesData);
    }
  },
  computed: {
    getDate() {
      return new Date()
        .toLocaleDateString("ru", {
          month: "long",
          day: "numeric",
          year: "numeric",
        })
        .slice(0, -3);
    },
  },
  methods: {
    sinitizer(string) {
      const UNSAFE_CHARS_RE = /<|>\/|'|\u2028|\u2029/g;
      const ESCAPED_CHARS = {
        "<": "&lt;",
        ">": "&gt;",
        '"': "&quot;",
        "'": "\\u0027",
        "</": "<\\u002F",
        "\u2028": "\\u2028",
        "\u2029": "\\u2029",
      };
      const escapeUnsafeChars = (unsafeChar) => ESCAPED_CHARS[unsafeChar];
      return string.replace(UNSAFE_CHARS_RE, escapeUnsafeChars);
    },
    onSubmit() {
      if (this.userMessage) {
        const userData = {
          name: this.userName,
          date: this.getDate,
          message: this.sinitizer(this.userMessage),
        };
        this.messages.push(userData);
        this.userMessage = "";
        localStorage.setItem("chat-messages", JSON.stringify(this.messages));
      }
    },
  },
};
</script>

<style scoped lang="scss">
.chat {
  max-width: 720px;
  min-width: 360px;
  margin: 0 auto;
}
.chat-container {
  width: 100%;
  margin: 0 auto;
  padding: 0 20px;
}
.chat-bg {
  background-color: #fff;
}
.chat-manager {
  padding-top: 14px;
}
.chat-manager__avatar {
  padding-left: 86px;
  position: relative;
  width: 124px;
}
.chat-manager__avatar-img {
  border-radius: 50%;
  position: absolute;
  top: 0;
  left: 0;
  z-index: 1;
}
.chat-manager__text {
  padding-left: 86px;
}
.chat-manager__name,
.chat-manager__position,
.chat-manager__quote {
  line-height: 20px;
  padding-left: 45px;
}
.chat-manager__name {
  padding-top: 7px;
  margin-bottom: 2px;
}
.chat-manager__position {
  font-size: 12px;
  margin-bottom: 5px;
  color: #808080;
  display: inline-block;
}
.chat-manager__quote {
  background-color: #fffbc8;
  border: 1px solid #dadada;
  border-radius: 5px;
  padding: 6px 12px 7px 45px;
}
.chat__services {
  margin: 13px 0 20px;
}
.chat-services {
  max-width: 318px;
  margin-left: auto;
}
.chat-services__service {
  font-size: 13px;
  line-height: 15px;
  display: block;
  text-align: right;
  margin-right: 34px;
}
.chat-services__inner {
  margin: 7px 0 9px;
  padding: 16px 40px 15px 0;
  border-top: 1px solid #dadada;
  border-bottom: 1px solid #dadada;
}
.chat-services__items {
  position: relative;
  &::before {
    content: "";
    position: absolute;
    top: 0;
    left: 0;
    width: 1px;
    height: 100%;
    background-color: rgba(51, 51, 51, 0.2);
  }
}
.chat-services__item {
  border-left: 1px solid rgba(#333, 0.2);
  display: flex;
  justify-content: space-between;
  align-items: center;
  position: relative;
  &:not(:last-child) {
    margin-bottom: 2px;
  }
  &::before {
    position: absolute;
    content: "";
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background-color: #b1e19b;
    max-width: 220px;
    border-radius: 0px 3px 3px 0px;
  }
  &:nth-child(2)::before {
    background-color: #ace2f8;
    max-width: 55px;
  }
  &:nth-child(3)::before {
    background-color: #ace2f8;
    max-width: 35px;
  }
}
.services-item-name {
  font-size: 13px;
  line-height: 26px;
  padding: 0 6px;
  z-index: 1;
}
.services-item-amount {
  font-weight: bold;
  font-size: 13px;
  line-height: 15px;
}
.chat-services__total {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding-right: 40px;
}
.chat-services__total-text {
  line-height: 26px;
}
.chat-services__total-num {
  line-height: 15px;
}
.chat__feedback {
  margin-bottom: 8px;
}
.chat-feedback {
  display: flex;
}
.chat-feedback__last {
  line-height: 16px;
  margin-right: 5px;
}
.chat-feedback__link {
  line-height: 16px;
  text-decoration: underline;
  color: #005da1;
  transition: all 0.3s;
  margin-right: 5px;
  &:hover,
  &:active {
    color: #fdd639;
  }
}
.chat-feedback__icons {
  margin-left: auto;
}
.chat-feedback__icon {
  font-size: 12px;
  line-height: 14px;
  position: relative;
  &:not(:last-child) {
    margin-right: 18px;
  }
  &::before {
    position: absolute;
    left: 0;
  }
}
.chat-feedback__icon-like {
  padding-left: 16px;
  &::before {
    top: 2px;
    content: url(../assets/icon-like.svg);
  }
}
.chat-feedback__icon-comment {
  padding-left: 17px;
  &::before {
    top: 3px;
    content: url(../assets/icon-comment.svg);
  }
}
.chat-messages {
  padding-bottom: 22px;
}
.chat-messages__item {
  &:not(:last-child) {
    margin-bottom: 16px;
  }
}
.chat-messages__header {
  margin-bottom: 9px;
}
.chat-messages__user-name {
  font-weight: bold;
  font-size: 14px;
}
.chat-messages__date {
  margin-left: 8px;
  font-weight: 400;
  font-size: 11px;
  color: #808080;
}
.chat-messages__message {
  padding: 17px 15px 17px 20px;
  background-color: #f2fbff;
  border: 1px solid #c4cbcf;
  position: relative;
  box-shadow: 0px 4px 10px rgba(128, 128, 128, 0.1);
  &::before,
  &::after {
    content: " ";
    position: absolute;
    width: 0;
    height: 0;
  }
  &::before {
    top: -8px;
    left: 23px;
    border: 4px solid;
    border-color: transparent transparent #c4cbcf #c4cbcf;
  }
  &::after {
    top: -6px;
    left: 24px;
    border: 3px solid;
    border-color: transparent transparent #f2fbff #f2fbff;
  }
}
.chat-form-bg {
  background-color: #f2f2f2;
}
.chat__form {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 28px 0 34px;
}
.chat__form-text {
  resize: none;
  width: 100%;
  background-color: #ffffff;
  border: 1px solid #000000;
  border-radius: 1px;
  margin-bottom: 23px;
  height: 63px;
  padding: 17px 15px 15px 20px;
  font-family: Arial, sans-serif;
  font-weight: 400;
  font-size: 14px;
  line-height: 19px;
  color: $text;
  transition: all 0.3s;
  &:hover {
    border: 1px solid #fdd639;
  }
}
.chat__form-error {
  display: none;
}
.chat__form-btn {
  background-color: #fdd639;
  border-radius: 23px;
  line-height: 21px;
  font-family: "PT Sans", sans-serif;
  max-width: 280px;
  width: 100%;
  padding: 12px 13px 10px;
  transition: all 0.3s;
  &:hover,
  &:active {
    background-color: $text;
    color: #fdd639;
  }
}

@media (max-width: 425px) {
  .chat-feedback__icon {
    display: block;
    &:not(:last-child) {
      margin-right: 0;
    }
  }
}
</style>
