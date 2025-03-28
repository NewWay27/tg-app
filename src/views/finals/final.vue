<template>
    <div class="desktop-background">
        <img src="@/assets/logo_desktop.png" alt="logo" class="logo-desktop">
        <div class="tg-desktop">
            <span>Телеграм-канал</span>
            <img src="@/assets/tg-d.svg" alt="tg">
        </div>
        <div class="mobile-window">
            <div class="page">
                <div class="header">
                    <img src="@/assets/logo2.svg" alt="logo">
                    <div class="header__menu" @click="openMenu">
                        <img src="@/assets/hamb.svg" alt="menu">
                        <span>Меню</span>
                    </div>
                </div>

                <div class="text" v-show="showText">
                    <span>
                        Поздравляем, игра пройдена!
                    </span>
                    <!-- <span>
                        Чтобы участвовать в розыгрыше, осталось только подписаться на телеграм Ozon Tech. Там много
                        полезного и
                        немного смешного.
                    </span>
                    <span>
                        А ещё у нашего бота есть статьи про упомянутые <br> в игре решения.
                    </span> -->

                    <span>
                        За набранные баллы ты можешь получить наш мерч на стенде.
                    </span>
                    <span>
                        А если хочешь попробовать процессы из игры в реальной жизни, жми на кнопку ниже.
                    </span>
                </div>

                <img src="@/assets/hello.png" alt="logo" class="goose">
                <div class="button" @click="openVacancies">
                    <span>Вакансии в Ozon Tech</span>
                    <img src="@/assets/op.svg" alt="go">
                </div>
            </div>

            <TheMenu v-if="showMenu" @close="showMenu = false">
                <template #title></template>
                <template #body>
                    <div class="menu">
                        <div @click="openChat">
                            <img src="@/assets/reload.svg" alt="reload">
                            <span>Сыграть ещё раз</span>
                        </div>
                        <div @click="openLeaderboard">
                            <img src="@/assets/coin.svg" alt="coin">
                            <span>Открыть таблицу лидеров</span>
                        </div>
                        <div @click="openTelegram">
                            <img src="@/assets/tg.svg" alt="tg">
                            <span>Перейти в телеграм-канал Ozon Tech</span>
                        </div>
                    </div>
                </template>
            </TheMenu>
        </div>
    </div>
</template>

<script>
import TheMenu from '@/components/TheMenu.vue';

export default {
    components: {
        TheMenu
    },
    data() {
        return {
            showText: false,
            showMenu: false,
        };
    },
    mounted() {
        document.body.style.overflow = 'hidden';
        setTimeout(() => {
            this.showText = true;
        }, 800);
    },
    methods: {
        // openVacancies() {
        //     window.location.href = 'https://ozon.tech/gamebot-job';
        // },
        getCookie(name) {
            const matches = document.cookie.match(new RegExp(
                `(?:^|; )${name.replace(/([$?*|{}()[\]\\/+^])/g, '\\$1')}=([^;]*)`
            ));
            return matches ? decodeURIComponent(matches[1]) : null;
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

        async openVacancies() {
            // window.location.href = 'https://ozon.tech/gamebot-job';

            try {
                // Получаем CSRF-токен и UUID из cookies
                await this.fetchCsrfToken(); // Дожидаемся завершения получения CSRF-токена
                const csrfToken = this.getCookie('XSRF-TOKEN');
                const user_id = this.getCookie('user_id');
                const sessionId = this.getCookie('X-Session-ID');
                const utm_source = localStorage.getItem('utm_source')
                const utm_medium = localStorage.getItem('utm_medium')
                const utm_campaign = localStorage.getItem('utm_campaign')
                // Проверяем наличие CSRF-токена и UUID
                if (!csrfToken) {
                    console.error('CSRF-токен не найден в куках.');
                    return;
                }
                if (!user_id) {
                    console.error('user_id не найден в куках.');
                    return;
                }

                // Формируем URL для запроса
                const url = new URL(`https://ozon.tech/gamebot-job`);
                // url.searchParams.append('utm_source', 'tg');
                // url.searchParams.append('utm_medium', 'gamebot');
                // url.searchParams.append('utm_campaign', user_id);
                url.searchParams.append('utm_source', utm_source);
                url.searchParams.append('utm_medium', utm_medium);
                url.searchParams.append('utm_campaign', utm_campaign);

                // Устанавливаем заголовки
                const headers = {
                    "Content-Type": "application/json",
                    "Accept": "application/json",
                    'X-Session-ID': sessionId,
                    "X-XSRF-TOKEN": csrfToken,
                    "Sec-Fetch-Dest": "empty",
                    "Sec-Fetch-Mode": "cors",
                    "Sec-Fetch-Site": "same-origin", // Используется, если фронтенд и API на одном домене
                };

                // Выполняем POST-запрос
                const response = await fetch(url.toString(), {
                    method: "POST",
                    headers,
                    credentials: "include", // Включаем cookies в запрос
                });

                // Проверяем статус ответа
                if (!response.ok) {
                    throw new Error(`Ошибка запроса: ${response.status} ${response.statusText}`);
                }

                // Обрабатываем успешный ответ
                const data = await response.json();
                console.log('Данные успешно отправлены:', data);

                // Переход на другую страницу
                window.location.href = url;
            } catch (error) {
                // Логируем ошибку
                console.error('Ошибка при отправке данных:', error);
            }
        },
        openMenu() {
            this.showMenu = true;
            document.body.style.overflow = 'hidden';
        },
        openLeaderboard() {
            // this.$router.push('/finals/leaderboard');
            this.$router.push('/door');
            document.body.style.overflow = '';
        },
        openChat() {

            this.$router.push('/chat/obx');
            document.body.style.overflow = '';
        },
        openTelegram() {
            window.location.href = 'https://t.me/s/ozon_tech';
        }
    }
}
</script>

<style lang="scss" scoped>
.desktop-background {
    background: url('@/assets/bgg.svg') no-repeat center center;
    background-size: cover;
    display: flex;
    justify-content: center;
    align-items: end;
    height: 100vh;

    @media (max-width: 480px) {
        background: none;
    }

    .logo-desktop {
        position: absolute;
        right: 100px;
        top: 100px;

        @media (max-width: 1440px) {
            right: 50px;
            top: 50px;
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
            left: 30px;
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
    overflow: hidden;
    border-radius: 29px 29px 0 0;
    box-shadow: 0 0 14px 0 #032b44;

    @media (max-width: 1440px) {
        width: 390px;
    }

    @media (max-width: 480px) {
        width: 100%;
        border-radius: 0;
    }
}

.page {
    overflow: hidden;
    position: relative;
    height: 100vh;
    background-color: #02283E;
    background-image: url(@/assets/Vector.png);
    background-size: cover;
    padding: 0 17px;

    @media (max-width: 1920px) {
        height: 90vh;
    }

    @media (max-width: 800px) {
        height: 100vh;
    }
}

.header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    border-bottom: 0.67px solid rgba(255, 255, 255, 0.2);
    border-radius: 0 0 13px 13px;
    background: #0a3956;
    z-index: 100;
    position: fixed;
    top: 0;
    left: 0;
    right: 0;

    @media (max-width: 1920px) {
        position: static;
        margin: 0 -17px;
    }

    @media (max-width: 800px) {
        position: fixed;
        margin: 0;
    }

    &__menu {
        border-radius: 25px;
        padding: 6px 12px;
        height: 33px;
        background: #005bff;
        display: flex;
        gap: 7px;
        align-items: center;
        margin: 0 20px 0 0;

        span {
            font-family: var(--gte);
            font-weight: 400;
            font-size: 16px;
            color: #fff;
        }
    }
}

.goose {
    width: 254px;
    position: absolute;
    bottom: 60px;
    left: 80px;

    @media (max-width: 1920px) {
        bottom: 60px;
        left: 150px;
    }

    @media (max-width: 1440px) {
        bottom: 60px;
        left: 80px;
    }

    @media (max-width: 800px) {
        left: 300px;
        width: 215px;
    }

    @media (max-width: 600px) {
        left: 130px;
        width: 254px;
    }

    @media (max-width: 430px) {
        left: 80px;
    }

    @media (max-width: 425px) {
        width: 225px;
        left: 120px;
    }

    @media (max-width: 420px) {
        width: 254px;
        left: 80px;
    }
}

.button {
    border-radius: 5px;
    padding: 11px 13px;
    box-shadow: 0 4px 0 0 #054fd4;
    background: #005bff;
    display: flex;
    justify-content: space-between;
    align-items: center;

    position: absolute;
    bottom: 60px;
    left: 17px;
    right: 17px;

    span {
        font-family: var(--gte);
        font-weight: 400;
        font-size: 20px;
        color: #fff;
    }
}

.text {
    border-radius: 7px;
    padding: 10px 13px;
    backdrop-filter: blur(6.741573333740234px);
    background: #fff;
    text-align: center;
    margin: 120px 0 0 0;
    display: flex;
    flex-direction: column;
    gap: 10px;
    opacity: 0;
    animation: slideDown 1s forwards;

    @media (max-width: 1920px) {
        animation: slideDownDesktop 1s forwards;
    }

    @media (max-width: 800px) {
        animation: slideDown 1s forwards;
    }

    span {
        font-family: var(--gte);
        font-weight: 400;
        font-size: 14px;
        text-align: center;
        color: #073049;
    }
}

.menu {
    display: flex;
    flex-direction: column;
    gap: 27px;

    div {
        display: flex;
        gap: 13px;
        align-items: center;
        cursor: pointer;

        span {
            font-family: var(--gte);
            font-weight: 400;
            font-size: 16px;
            color: #fff;
        }
    }
}

@keyframes slideDown {
    0% {
        opacity: 0;
        margin: 50px 0 0 0;
    }

    100% {
        margin: 120px 0 0 0;
        opacity: 1;
    }
}

@keyframes slideDownDesktop {
    0% {
        opacity: 0;
        margin: 50px 0 0 0;
    }

    100% {
        margin: 50px 0 0 0;
        opacity: 1;
    }
}
</style>
