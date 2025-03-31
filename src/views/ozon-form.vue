<template>
  <div class="desktop-background">
    <img src="@/assets/logo_desktop.png" alt="logo" class="logo-desktop">
    <div class="tg-desktop" @click="openTelegram">
      <span>Телеграм-канал</span>
      <img src="@/assets/tg-d.svg" alt="tg">
    </div>
    <div class="mobile-window">
      <div class="header">
        <img src="@/assets/arr.svg" class="header__back" v-if="!showBtn" @click="goBack">
        <img src="@/assets/logo-ozon.svg" class="header__logo">
      </div>

      <div class="content">
        <iframe id="ozon-form" src="https://forms.ozon.tech/form/149237e97f3389bab857dc4d8b209d1efa?embed=true"
          ref="ozonForm"></iframe>
        <button v-if="showBtn" @click="goToGame">Продолжить</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      showBtn: false,
    }
  },
  mounted() {
    document.body.style.overflow = 'hidden';
    // this.fetchCsrfToken();

    // Подписка на событие для отслеживания загрузки формы
    window.addEventListener('message', this.handleFormLoaded);
  },
  methods: {
    goBack() {
      this.$router.push('/');
    },
    handleFormLoaded({ data }) {
      // Проверяем, что событие - это загрузка формы
      if (data && data.event === 'ozon-forms:form-loaded') {
        console.log("Форма загружена, устанавливаем фон");
        this.setBackgroundColor();
      }
      if (data && data.event === 'ozon-forms:submit-form') {
        this.showBtn = true;
      }
    },
    openTelegram() {
      window.open('https://t.me/s/ozon_tech', '_blank', 'noopener,noreferrer');
    },
    setBackgroundColor() {
      const iframe = this.$refs.ozonForm;
      const uuid = this.getCookie('uuid');
      console.log(uuid)

      if (iframe && iframe.contentWindow) {
        iframe.contentWindow.postMessage({
          event: 'ozon-forms:action-set-css-variable',
          name: '--custom-header-display',
          value: 'none'
        }, '*');
        iframe.contentWindow.postMessage({
          event: 'ozon-forms:action-set-font',
          font: 'GT Eesti Pro Display'
        }, '*');
        iframe.contentWindow.postMessage({
          event: 'ozon-forms:action-success-screen',
          title: 'Рады знакомству! ',
          caption: 'А теперь вперёд к игре ➡️',
        }, '*');
        iframe.contentWindow.postMessage({
          event: 'ozon-forms:utm-marks-message',
          utmContent: uuid,
          utmSource: 'ozonhrbot',
        }, '*');
      } else {
        console.error("Не удалось получить доступ к contentWindow iframe");
      }
    },
    async fetchCsrfToken() {
      try {
        const response = await fetch('https://korobchik.ozon.tech/api/sanctum/csrf-cookie', {
          method: 'GET',
          credentials: 'include', // Включает отправку и получение cookies
          headers: {
            'Accept': 'application/json',
          },
        });

        // if (!response.ok) {
        //     throw new Error(`Ошибка получения CSRF-токена: ${response.status} ${response.statusText}`);
        // }

        // const data = await response.json(); // Парсим JSON-ответ

        // if (!data.csrfToken) {
        //     throw new Error("CSRF-токен отсутствует в ответе.");
        // }

        // console.log('CSRF Token:', data.csrfToken);

        // Устанавливаем CSRF-токен в куки браузера (на 2 часа)
        // document.cookie = `XSRF-TOKEN=${data.csrfToken}; path=/; max-age=7200; secure; SameSite=Lax`;
        // document.cookie = `X-Session-ID=${data.sessionId}; path=/; max-age=7200; secure; SameSite=Lax`;
        console.log("CSRF-токен успешно сохранен в куки.");
        // return data.csrfToken; // Можно вернуть токен, если он нужен в коде

      } catch (error) {
        console.error('Ошибка при запросе CSRF-токена:', error.message);
      }
    },

    getCookie(name) {
      const matches = document.cookie.match(new RegExp(
        `(?:^|; )${name.replace(/([$?*|{}()[\]\\/+^])/g, '\\$1')}=([^;]*)`
      ));
      return matches ? decodeURIComponent(matches[1]) : null;
    },
    async goToGame() {
      try {
        // Дожидаемся установки CSRF-токена
        await this.fetchCsrfToken();
        const sessionId = this.getCookie('X-Session-ID');
        const csrfToken = this.getCookie('XSRF-TOKEN');
        const uuid = this.getCookie('uuid');

        // Проверяем наличие токенов
        if (!csrfToken) {
          console.error('CSRF-токен не найден в куках.');
          return;
        }

        if (!uuid) {
          console.error("Куки с именем 'uuid' не найдены.");
          return;
        }

        console.log('UUID:', uuid);

        const url = new URL('https://ozontechhrbot.ru/api/form/create'); // Создаем URL-объект

        // Формируем заголовки
        const headers = {
          'Content-Type': 'application/json',
          'Accept': 'application/json',
          'X-Session-ID': sessionId,
          "X-XSRF-TOKEN": csrfToken,
          "Sec-Fetch-Dest": "empty",
          "Sec-Fetch-Mode": "cors",
          "Sec-Fetch-Site": "same-origin",
        };

        // Отправляем POST-запрос
        const response = await fetch(url.toString(), {
          method: 'POST',
          headers, // Подставляем заранее сформированные заголовки
          credentials: 'include', // Включаем cookies в запрос
          body: JSON.stringify({ code: uuid }), // Тело запроса
        });

        if (!response.ok) {
          throw new Error(`Ошибка HTTP: ${response.status} ${response.statusText}`);
        }

        const data = await response.json(); // Парсим ответ в JSON
        console.log('Ответ сервера:', data);
        this.$router.push('/start/last-screen');
      } catch (error) {
        console.error('Ошибка при выполнении запроса:', error);
      }
    }
  }
};
</script>
<style lang="scss" scoped>
.desktop-background {
  background: url('@/assets/bgg.svg') no-repeat center center;
  background-size: cover;
  display: flex;
  justify-content: center;
  align-items: end;
  height: 100dvh;

  @media (max-width: 480px) {
    background: none;
  }

  .logo-desktop {
    position: absolute;
    right: 100px;
    top: 100px;

    @media (max-width: 1440px) {
      top: 80px;
    }

    @media (max-width: 480px) {
      display: none;
    }
  }

  .tg-desktop {
    position: absolute;
    left: 100px;
    bottom: 50px;
    border-radius: 31px;
    padding: 13px 25px;
    background: #005bff;
    display: flex;
    gap: 13px;
    justify-content: space-between;
    align-items: center;
    cursor: pointer;

    @media (max-width: 1440px) {
      bottom: 30px;
    }

    @media (max-width: 480px) {
      display: none;
    }

    span {
      font-family: var(--gte);
      font-weight: 400;
      font-size: 23px;
      color: #fff;
    }
  }
}

.mobile-window {
  width: 539px;
  position: relative;
  border-radius: 29px 29px 0 0;
  box-shadow: 0 0 14px 0 #032b44;

  &::-webkit-scrollbar {
    height: 0;
    width: 0;
  }

  @media (max-width: 1440px) {
    width: 390px;
  }

  @media (max-width: 480px) {
    width: 100%;
    border-radius: 0;
    box-shadow: none;
  }
}

.header {
  background-color: #F9FAFC;
  display: flex;
  gap: 45px;
  align-items: center;
  text-align: center;
  justify-content: center;
  // position: fixed;
  position: absolute;
  top: 0;
  left: 0;
  z-index: 5;
  width: 100%;

  &__logo {
    text-align: center;
  }

  &__back {
    position: absolute;
    left: 13px;
  }
}

.content {
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
  position: relative;
  background-color: #fff;
  height: 100dvh;

  iframe {
    border: none;
    width: 100%;
    height: 100dvh;
    background: #f9fafb;
    padding: 40px 0 0px;

    @media (max-width: 480px) {
      height: 100dvh;
    }
  }

  button {
    border-radius: 5px;
    background: #005bff;
    font-family: var(--gte);
    font-weight: 400;
    font-size: 20px;
    color: #fff;
    border: none;
    box-shadow: 0 4px 0 0 #054fd4;
    background: #005bff;
    padding: 13px 118px;
    // position: absolute;
    // bottom: 20px;
    // left: 13px;
    // right: 13px;
    z-index: 1000;
    margin: 0 13px 100px;
    white-space: nowrap;
    cursor: pointer;
  }
}
</style>
