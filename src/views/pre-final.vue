<template>
    <div class="desktop-background">
        <img src="@/assets/logo_desktop.png" alt="logo" class="logo-desktop">
        <div class="tg-desktop" @click="openTelegram">
            <span>Телеграм-канал</span>
            <img src="@/assets/tg-d.svg" alt="tg">
        </div>
        <div class="mobile-window">
            <!-- <TheLoader v-if="isLoading" /> -->
            <div class="page">
                <div class="gosha">
                    <!-- <TheLoader v-if="isLoading" /> -->
                    <img src="@/assets/open_eyes.png" alt="goose" class="gosha__img">

                    <div class="gosha__dialog" v-show="showDialog">
                        <img src="@/assets/dialog4.png" alt="dialog">
                        <div>
                            <span>Ты круто показал себя — справился <br> с высокими нагрузками и доставил <br>
                                Коробчика.</span>
                            <span>
                                Если хочешь работать над решениями, которыми пользуются миллионы, смотри вакансии на
                                нашем
                                сайте.
                            </span>
                        </div>
                    </div>

                    <div class="gosha__buttons">
                        <div class="gosha__buttons_blue" @click="openVacancies">
                            <span>Вакансии в Ozon Tech</span>
                            <img src="@/assets/op.svg" alt="go">
                        </div>
                        <div class="gosha__buttons_white" @click="goToFinal">
                            <span>Продолжить</span>
                            <img src="@/assets/dis.svg" alt="dis">
                        </div>
                    </div>

                </div>
            </div>
        </div>
    </div>
</template>

<script>
import TheLoader from '@/components/TheLoader.vue';

export default {
    components: {
        TheLoader
    },
    data() {
        return {
            isLoading: true,
            showDialog: false
        }
    },
    methods: {
        openTelegram() {
            window.open('https://t.me/s/ozon_tech', '_blank', 'noopener,noreferrer');
        },
        goToFinal() {
            this.$router.push('/finals/');
        },
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
                // Получаем CSRF-токен и user_id из cookies
                await this.fetchCsrfToken(); // Дожидаемся завершения получения CSRF-токена
                const csrfToken = this.getCookie('XSRF-TOKEN');
                const user_id = this.getCookie('user_id');

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
                url.searchParams.append('utm_source', 'tg');
                url.searchParams.append('utm_medium', 'gamebot');
                url.searchParams.append('utm_campaign', user_id);

                // Устанавливаем заголовки
                const headers = {
                    "Content-Type": "application/json",
                    "Accept": "application/json",
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
    },
    mounted() {
        setTimeout(() => {
            this.isLoading = false;
        }, 2000);
        setTimeout(() => {
            this.showDialog = true;
        }, 3000);
    },
}
</script>

<style lang="scss">
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
}

.gosha {
    background: url(@/assets/fon.png) no-repeat center fixed;
    background-size: cover;
    width: 100%;
    height: 100dvh;

    @media (max-width: 1920px) {
        height: 90vh;
        background-size: contain;
    }

    @media (max-width: 800px) {
        height: 100dvh;
        background-size: cover;
    }

    &__img {
        width: 100%;
        position: absolute;
        bottom: -70px;
        left: -90px;

        @media (max-width: 1920px) {
            bottom: -90px;
            left: -130px;
        }

        @media (max-width: 1440px) {
            bottom: -70px;
            left: -90px;
        }

        @media (max-width: 800px) {
            width: 400px;
        }

        @media (max-width: 430px) {
            left: -100px;
            width: 100%;
        }
    }

    &__dialog {
        position: absolute;
        top: 200px;
        left: 5px;

        @media (max-width: 1920px) {
            top: 60px;
            left: 40px;
        }

        @media (max-width: 1440px) {
            top: 100px;
            left: 0;
        }

        @media (max-width: 800px) {
            top: 10px;
            left: 90px;
        }

        @media (max-width: 600px) {
            top: 180px;
            left: 20px;
        }

        @media (max-width: 430px) {
            top: 130px;
            left: 20px;
        }

        @media (max-width: 425px) {
            top: 10px;
        }

        @media (max-width: 420px) {
            top: 120px;
            left: 0;
        }

        @media (max-width: 390px) {
            top: 100px;
            left: 10px;
        }

        @media (max-width: 375px) {
            top: 50px;
            left: 0;
        }

        img {

            @media (max-width: 1920px) {
                width: 460px;
            }

            @media (max-width: 1440px) {
                width: auto;
            }

            @media (max-width: 420px) {
                width: 420px;
            }

            @media (max-width: 390px) {
                width: auto;
            }
        }

        div {
            display: flex;
            flex-direction: column;
            gap: 3px;
            position: absolute;
            top: 18px;
            left: 25px;
            right: 15px;

            @media (max-width: 420px) {
                top: 15px;
            }

            @media (max-width: 390px) {
                top: 15px;
            }

            span {
                font-family: var(--gte);
                font-weight: 400;
                font-size: 16px;
                color: #073049;
                line-height: 105%;

                @media (max-width: 1920px) {
                    font-size: 20px;
                }

                @media (max-width: 1440px) {
                    font-size: 16px;
                }
            }
        }
    }

    &__buttons {
        display: flex;
        flex-direction: column;
        gap: 14px;
        position: absolute;
        bottom: 50px;
        left: 17px;
        right: 17px;
        opacity: 0;
        animation: slideUpBtn 5s forwards;

        &_blue {
            border-radius: 5px;
            padding: 11px 13px;
            box-shadow: 0 4px 0 0 #054fd4;
            background: #005bff;
            display: flex;
            gap: 98px;
            align-items: center;
            justify-content: space-between;
            white-space: nowrap;

            span {
                font-family: var(--gte);
                font-weight: 400;
                font-size: 20px;
                color: #fff;
            }
        }

        &_white {
            border-radius: 5px;
            padding: 11px 13px;
            box-shadow: 0 4px 0 0 #e4e1e6;
            background: #fff;
            display: flex;
            gap: 133px;
            align-items: center;
            justify-content: space-between;
            white-space: nowrap;

            span {
                font-family: var(--gte);
                font-weight: 400;
                font-size: 20px;
                color: #005bff;
            }
        }
    }
}

@keyframes slideUpBtn {
    0% {
        bottom: -100%;
        opacity: 0;
    }

    100% {
        bottom: 50px;
        opacity: 1;
    }
}
</style>
