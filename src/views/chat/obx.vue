<template>
    <div class="desktop-background">
        <img src="@/assets/logo_desktop.png" alt="logo" class="logo-desktop">
        <div class="tg-desktop">
            <span>Телеграм-канал</span>
            <img src="@/assets/tg-d.svg" alt="tg">
        </div>
        <div class="mobile-window">
            <div class="page">
                <TheHeaderChat />
                <div class="chat">
                    <div class="chat__right">
                        <div class="chat__right-bubble right-text-1">
                            <TheChatWhite>
                                <template #chattng__text>
                                    <span>Пора доставить подарок! Но на складе столько всего… боюсь запутаться. Хорошо,
                                        что
                                        с нами Гоша, он проводит.</span>
                                </template>
                            </TheChatWhite>
                        </div>
                        <div class="chat__right-avatar right-avatar-1">
                            <img src="@/assets/ava_b.png" alt="avatar" class="avatars">
                        </div>
                    </div>

                    <!-- <div class="chat__note left-note" v-show="showNote" @animationend="handleNoteAnimationEnd">
                <div class="chat__note-avatar">
                    <img src="@/assets/ava_g.png" alt="avatar" class="avatars">
                </div>
                <div class="chat__note-pen">
                    <img src="@/assets/pen.svg" alt="avatar" class="pen-animation">
                    <img src="@/assets/paper.svg" alt="avatar">
                </div>
            </div> -->

                    <div class="chat__left show-left-1" v-show="showLeft">
                        <div class="chat__left-avatar avatar-1">
                            <img src="@/assets/ava_g.png" alt="avatar" class="avatars">
                        </div>
                        <div class="chat__left-bubble">
                            <TheChatBlue :isLast="true">
                                <template #chattng__text>
                                    <div class="chat__left-bubble_inner">
                                        <img src="@/assets/chat1.png" alt="pic">
                                        <span>Ты прав! У нас здесь настоящий хайлоад. Но не волнуйся, доставим тебя в
                                            срок.</span>
                                    </div>
                                </template>
                            </TheChatBlue>
                        </div>
                    </div>

                    <!-- <div class="tap" v-if="showTap">
                <img src="@/assets/tap.svg" alt="tap" @click="addNewChat">
            </div> -->
                    <div class="tap-container" v-if="showTap" @click="addNewChat">
                        <img src="@/assets/tap.svg" alt="tap" class="tap">
                    </div>

                    <div class="chat__right" v-show="showNewChat">
                        <div class="chat__right-bubble right-text-2">
                            <TheChatWhite>
                                <template #chattng__text>
                                    <span>Здесь же тысячи посылок, как бы нам всем поместиться в фургоны для
                                        доставки?</span>
                                </template>
                            </TheChatWhite>
                        </div>
                        <div class="chat__right-avatar right-avatar-2">
                            <img src="@/assets/ava_b.png" alt="avatar" class="avatars">
                        </div>
                    </div>

                    <!-- <div class="chat__note left-note" v-show="showSecondNote" @animationend="handleSecondNoteAnimationEnd">
                <div class="chat__note-avatar">
                    <img src="@/assets/ava_g.png" alt="avatar" class="avatars">
                </div>
                <div class="chat__note-pen">
                    <img src="@/assets/pen.svg" alt="avatar" class="pen-animation">
                    <img src="@/assets/paper.svg" alt="avatar">
                </div>
            </div> -->

                    <div class="chat__left show-left-1" v-show="showLeftNewChat">
                        <div class="chat__left-avatar" ref="avatar">
                            <img src="@/assets/ava_g.png" alt="avatar" class="avatars">
                        </div>
                        <div class="chat__left-bubble">
                            <TheChatBlue v-for="(message, index) in chatMessages" :key="index"
                                v-show="showChatBlue[index]" :isLast="index === chatMessages.length - 1">
                                <template #chattng__text>
                                    <!-- <div class="chat__left-bubble_inner" :ref="'scrollAnchor' + index"> -->
                                    <div class="chat__left-bubble_inner">
                                        <span>{{ message }}</span>
                                    </div>

                                </template>
                            </TheChatBlue>
                        </div>
                    </div>

                    <!-- <div class="tap2" v-if="showTap2" ref="scrollAnchor10">
                <img src="@/assets/tap.svg" alt="tap" @click="addFinalChat">
            </div> -->

                    <div class="tap2-container" v-if="showTap2" @click="addFinalChat" ref="scrollAnchor10">
                        <img src="@/assets/tap.svg" alt="tap" class="tap2">
                    </div>

                    <div class="chat__right_2" v-show="showRightFinal">
                        <div class="chat__right-bubble right-text-3">
                            <TheChatWhite>
                                <template #chattng__text>
                                    <span>Ого, я везу что-то хрупкое!</span>
                                </template>
                            </TheChatWhite>
                        </div>
                        <div class="chat__right-avatar right-avatar-3">
                            <img src="@/assets/ava_b.png" alt="avatar" class="avatars">
                        </div>
                    </div>

                    <div ref="scrollAnchor11"></div>

                    <!-- <div class="chat__note left-note-final" v-show="showThirdNote" @animationend="handleSecondNoteAnimationEnd">
                <div class="chat__note-avatar">
                    <img src="@/assets/ava_g.png" alt="avatar" class="avatars">
                </div>
                <div class="chat__note-pen">
                    <img src="@/assets/pen.svg" alt="avatar" class="pen-animation">
                    <img src="@/assets/paper.svg" alt="avatar">
                </div>
            </div> -->
                    <div ref="scrollAnchor12"></div>

                    <div class="chat__left" v-show="showLeftFinal">
                        <div ref="avatar2"
                            style="opacity: 0; transform: translateY(0); transition: transform 0.5s ease, opacity 0.5s ease;">
                            <img src="@/assets/ava_g.png" alt="avatar" class="avatars">
                        </div>
                        <div class="chat__left-bubble">
                            <TheChatBlue v-for="(message, index) in chatFinalMessages" :key="index"
                                v-show="showChatBlueFinal[index]">
                                <template #chattng__text>
                                    <div class="chat__left-bubble_inner">
                                        <span>{{ message }}</span>
                                    </div>
                                </template>
                            </TheChatBlue>
                            <TheChatBlue :isLast="true" v-show="lastMessage">
                                <template #chattng__text>
                                    <div class="chat__left-bubble_inner">
                                        <img src="@/assets/obx.png" alt="pic">
                                        <span>А за измерение веса отвечают классические тензодатчики.</span>
                                    </div>
                                </template>
                            </TheChatBlue>
                        </div>
                    </div>
                    <div ref="scrollAnchor3"></div>

                    <div ref="scrollAnchor4">
                        <button class="chat__button" v-if="showButton" @click="goTo">Начать игру</button>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import TheChatWhite from '@/components/TheChatWhite.vue';
import TheHeaderChat from '@/components/TheHeaderChat.vue';
import TheChatBlue from '@/components/TheChatBlue.vue';
export default {
    components: {
        TheHeaderChat, TheChatWhite, TheChatBlue
    },
    data() {
        return {
            showNote: false,
            showLeft: false,
            showTap: false,
            showNewChat: false,
            showSecondNote: false,
            showThirdNote: false,
            showLeftNewChat: false,
            chatMessages: [
                "Поможет сканер объёмно-весовых характеристик.",
                "Он за 2 секунды измеряет габариты и вес любых товаров. Это помогает оптимально разместить заказы на складе и запланировать загрузку машин.",
                "Обычно товары складывают в тарные ящики, а особенно хрупкие — в коробки."
            ],
            showChatBlue: [false, false, false],
            showTap2: false,
            showRightFinal: false,
            showLeftFinal: false,
            chatFinalMessages: [
                "Сканер, как и большинство других решений, на которых строится продукт, инженеры Ozon Tech разрабатывают сами.",
                "Для измерения габаритов предметов мы используем технологии машинного зрения и нейронных сетей.",
            ],
            showChatBlueFinal: [false, false],
            showButton: false,
            lastMessage: false,
        };
    },
    mounted() {
        document.body.style.overflow = '';

        setTimeout(() => {
            this.showNote = true;
        }, 500);

        setTimeout(() => {
            // this.showNote = false;
            this.showLeft = true;
        }, 2000);

        setTimeout(() => {
            this.showTap = true;
        }, 5000);
    },
    methods: {
        setCookie(name, value, days) {
            const date = new Date();
            date.setTime(date.getTime() + days * 24 * 60 * 60 * 1000);
            const expires = `expires=${date.toUTCString()}`;
            document.cookie = `${name}=${value};${expires};path=/`;
        },
        getCookie(name) {
            const matches = document.cookie.match(new RegExp(
                `(?:^|; )${name.replace(/([$?*|{}()[\]\\/+^])/g, '\\$1')}=([^;]*)`
            ));
            return matches ? decodeURIComponent(matches[1]) : null;
        },
        scrollToElement(refName) {
            this.$nextTick(() => {
                const chat = document.querySelector('.chat');
                if (chat) {
                    chat.scrollTo({
                        top: chat.scrollHeight + 100,
                        behavior: 'smooth'
                    });
                }
            });
        },
        handleNoteAnimationEnd() {
            this.showNote = false;
        },
        addNewChat() {
            this.showNewChat = true;
            this.showTap = false;
            setTimeout(() => {
                this.showSecondNote = true;
            }, 1000);
            setTimeout(() => {
                this.showSecondNote = false;
                this.showLeftNewChat = true;
                this.showChatMessages();
            }, 1000);
        },
        // handleSecondNoteAnimationEnd() {
        //     this.showSecondNote = false;
        //     // el.classList.add('fadeout');
        //     let el = this.$refs.avatar;
        //     setTimeout(() => {
        //         el.classList.add('avatar-padding');
        //     }, 0);
        //     setTimeout(() => {
        //         el.classList.add('avatar-padding-2');
        //     }, 500);
        //     setTimeout(() => {
        //         el.classList.add('avatar-padding-3');
        //     }, 1000);  //3 4 5
        // },
        showChatMessages() {
            setTimeout(() => {
                const avatar = this.$refs.avatar;
                this.chatMessages.forEach((message, index) => {
                    setTimeout(() => {
                        // Отображаем сообщение
                        this.showChatBlue[index] = true;

                        // Отображаем аватар и синхронизируем его движение с сообщением
                        if (avatar) {
                            avatar.style.transition = 'transform 0.5s ease, opacity 0.5s ease';
                            avatar.style.opacity = '1'; // Убедитесь, что аватар виден
                            avatar.style.transform = `translateY(${index * 90}px)`; // Перемещение вниз
                        }

                        // Скроллим вниз
                        this.scrollToElement(`scrollAnchor${index}`);
                    }, index * 1000); // Задержка между появлениями сообщений и аватара
                });
            }, 1000);

            setTimeout(() => {
                this.showTap2 = true;
                this.scrollToElement("scrollAnchor10");
            }, 4000);
        },
        addFinalChat() {
            this.showRightFinal = true;
            this.showTap2 = false;
            this.scrollToElement("scrollAnchor11");
            setTimeout(() => {
                this.showThirdNote = true;
                this.scrollToElement("scrollAnchor12");
            }, 3000);
            setTimeout(() => {
                this.showThirdNote = false;
                this.showLeftFinal = true;
                this.showFinalMessages();
            }, 1000);
        },
        showFinalMessages() {
            let el = this.$refs.avatar2;
            // setTimeout(() => {
            //     el.classList.add('avatar-padding-4');
            // }, 500);
            // setTimeout(() => {
            //     el.classList.add('avatar-padding-5');
            // }, 3000);
            // setTimeout(() => {
            //     el.classList.add('avatar-padding-6');
            // }, 4000);  //2 3 3

            setTimeout(() => {
                this.chatFinalMessages.forEach((message, index) => {
                    setTimeout(() => {
                        this.showChatBlueFinal[index] = true;
                        if (el) {
                            el.style.opacity = '1';
                            el.classList.add('avatar-padding-4');
                            el.style.transition = 'transform 0.5s ease, opacity 0.5s ease';
                            // Убедитесь, что аватар виден
                            el.style.transform = `translateY(${index * 100}px)`; // Перемещение вниз
                        }
                        this.scrollToElement("scrollAnchor3");
                    }, index * 1000);
                });
            }, 2000);
            setTimeout(() => {
                let el = this.$refs.avatar2;
                const finalIndex = this.chatFinalMessages.length;
                this.showButton = true;
                this.lastMessage = true;
                if (el) {
                    el.style.transition = 'transform 0.5s ease, opacity 0.5s ease';
                    el.style.opacity = '1'; // Убедитесь, что аватар виден
                    el.style.transform = `translateY(${finalIndex * 175}px)`; // Перемещение вниз
                }
                this.scrollToElement("scrollAnchor4");
            }, 4000);
        },
        async goTo() {
            try {
                // Получаем CSRF-токен и UUID из cookies
                await this.fetchCsrfToken(); // Дожидаемся завершения получения CSRF-токена
                const csrfToken = this.getCookie('XSRF-TOKEN');
                const uuid = this.getCookie('uuid');
                const sessionId = this.getCookie('X-Session-ID');
                // Проверяем наличие CSRF-токена и UUID
                if (!csrfToken) {
                    console.error('CSRF-токен не найден в куках.');
                    return;
                }
                if (!uuid) {
                    console.error('UUID не найден в куках.');
                    return;
                }

                // Формируем URL для запроса
                const url = new URL("https://ozontechhrbot.ru/api/leaderboard/create");
                url.searchParams.append('code', uuid);

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
                window.location.href = '/games/tetris';
            } catch (error) {
                // Логируем ошибку
                console.error('Ошибка при отправке данных:', error);
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
        box-shadow: none;
    }
}

.page {
    background-color: #02283E;
    // height: 100dvh;
    background-image: url(@/assets/Vector.png);
    background-position: center;
    background-size: cover;
    background-repeat: no-repeat;
    // overflow-y: auto;


    &::-webkit-scrollbar {
        height: 0;
        width: 0;
    }

    @media (max-width: 1920px) {
        height: 90vh;
    }

    @media (max-width: 800px) {
        height: 100dvh;
    }
}

.chat {
    padding: 80px 7px 50px;
    margin: 0 0 20px 0;
    height: 70vh;
    overflow: auto;

    &::-webkit-scrollbar {
        height: 0;
        width: 0;
    }

    @media (max-width: 1920px) {
        padding: 0 7px 50px;
    }

    @media (max-width: 800px) {
        padding: 0 7px 50px;
    }

    &__button {
        border-radius: 5px;
        box-shadow: 0 4px 0 0 #054fd4;
        background: #005bff;
        font-family: var(--gte);
        font-weight: 400;
        font-size: 20px;
        text-align: center;
        color: #fff;
        border: none;
        padding: 11px 124px;
        width: 100%;
        margin: 65px 0 0 0;
        white-space: nowrap;
        cursor: pointer;
    }

    &__right {
        display: flex;
        gap: 5px;
        justify-content: end;
        align-items: end;
        margin: 13px 0 0 0;

        span {
            font-family: var(--gte);
            font-weight: 400;
            font-size: 14px;
            line-height: 120%;
            color: #000;
            position: relative;
            z-index: 100;
        }

        &-bubble {
            display: flex;
            justify-content: center;
            opacity: 0;
            margin: 0 0 20px 0;
        }

        &-avatar {
            display: flex;
            justify-content: flex-end;
            opacity: 0;
        }
    }

    &__right_2 {
        display: flex;
        gap: 5px;
        justify-content: end;
        align-items: end;
        margin: 48px 0 0 0;

        span {
            font-family: var(--gte);
            font-weight: 400;
            font-size: 14px;
            line-height: 120%;
            color: #000;
            position: relative;
            z-index: 100;
        }

        &-bubble {
            display: flex;
            justify-content: center;
            opacity: 0;
            margin: 0 0 20px 0;
        }

        &-avatar {
            display: flex;
            justify-content: flex-end;
            opacity: 0;
        }
    }

    &__note {
        display: flex;
        gap: 17px;
        margin: 13px 0 0 0;
        opacity: 0;

        &-pen {
            display: flex;
            flex-direction: column;
            gap: 1.35px;
        }
    }

    &__left {
        display: flex;
        gap: 5px;
        justify-content: start;
        align-items: start;
        margin: 13px 0 0 0;

        &-avatar {
            padding: 30px 0 0 0;
        }

        &-bubble {
            display: flex;
            flex-direction: column;
            gap: 4px;
            align-items: center;

            &_inner {
                display: flex;
                flex-direction: column;
                gap: 3px;

                img {
                    border-radius: 4px;
                }

                span {
                    font-family: var(--gte);
                    font-weight: 400;
                    font-size: 14px;
                    line-height: 120%;
                    color: #fff;
                    position: relative;
                    z-index: 100;
                }
            }
        }
    }

    .tap {
        position: fixed;
        bottom: 47px;
        right: 23px;
        animation: heartbeat 1.4s infinite;
        animation-delay: 1s;
        cursor: pointer;

        @media (max-width: 1920px) {
            right: 730px;
        }

        @media (max-width: 1440px) {
            right: 550px;
        }

        @media (max-width: 800px) {
            right: 23px;
        }
    }

    .tap2 {
        // position: fixed;
        // bottom: 0;
        // right: 13px;
        animation: heartbeat 2s infinite;
        animation-delay: 1s;
        cursor: pointer;
        display: inline-block;
        float: right;
        padding: 0 13px 10px 0;
        margin: 20px 0px 0px 0px;

        @media (max-width: 1920px) {
            padding: 0 0 60px 0;
        }

        @media (max-width: 800px) {
            padding: 0 13px 10px 0;
        }
    }

    .avatars {
        width: 40px;
    }
}

@keyframes fadeIn {
    0% {
        opacity: 0;
        transform: translateY(10px);
    }

    100% {
        opacity: 1;
        transform: translateY(0);
    }
}

@keyframes fadeOut {
    0% {
        opacity: 1;
    }

    100% {
        opacity: 0;
    }
}

@keyframes penMove {
    0% {
        transform: translateX(0);
    }

    50% {
        transform: translateX(15px);
    }

    100% {
        transform: translateX(0);
    }
}

@keyframes heartbeat {

    0%,
    20%,
    80%,
    100% {
        transform: scale(1);
    }

    50% {
        transform: scale(1.2);
    }

    60% {
        transform: scale(1.2);
    }

    70% {
        transform: scale(1.2);
    }
}

.right-avatar-1 {
    animation: fadeIn 0.3s ease-in forwards;
    animation-delay: 1s;
}

.right-text-1 {
    animation: fadeIn 0.3s ease-in forwards;
    animation-delay: 1s;
    opacity: 0;
}

.left-note {
    animation: fadeOut 0.3s ease-in forwards;
    animation-delay: 1s;
    opacity: 0;
}

.left-note-final {
    animation: fadeOut 1s ease-in forwards;
    animation-delay: 1s;
    opacity: 1;
}

.show-left-1 {
    animation: fadeIn 0.3s ease-in forwards;
    animation-delay: 1s;
    opacity: 0;
}

.right-avatar-2 {
    animation: fadeIn 0.3s ease-in forwards;
    animation-delay: 1s;
}

.right-text-2 {
    animation: fadeIn 0.3s ease-in forwards;
    animation-delay: 1s;
}

.right-avatar-3 {
    animation: fadeIn 0.3s ease-in forwards;
    animation-delay: 1s;
}

.right-text-3 {
    animation: fadeIn 0.3s ease-in forwards;
    animation-delay: 1s;
}

.pen-animation {
    animation: penMove 0.5s ease-in-out infinite;
}

.avatar-1 {
    padding: 260px 0 0 0;
}

.avatar-3 {
    padding: 420px 0 0 0;
}

.avatar-padding {
    padding: 40px 0 0 0;
}

.avatar-padding-2 {
    padding: 140px 0 0 0;
}

.avatar-padding-3 {
    padding: 160px 0 0 0;
}

.avatar-padding-4 {
    padding: 40px 0 0 0;
}

.avatar-padding-5 {
    padding: 110px 0 0 0;
}

.avatar-padding-6 {
    padding: 300px 0 0 0;
}

.tap-container {
    position: fixed;
    top: 0;
    left: 0;
    width: 100vw;
    height: 100dvh;
    display: flex;
    justify-content: center;
    align-items: center;
    z-index: 1000;
    cursor: pointer;
}

.tap2-container {
    position: fixed;
    bottom: 0;
    right: 0;
    width: 100vw;
    height: 100dvh;
    display: flex;
    justify-content: flex-end;
    align-items: flex-end;
    z-index: 1000;
    cursor: pointer;

    @media (max-width: 1920px) {
        right: 730px;
    }

    @media (max-width: 800px) {
        right: 0;
    }
}
</style>
