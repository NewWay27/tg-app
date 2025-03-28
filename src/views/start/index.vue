<template>

    <div class="desktop-background">
        <img src="@/assets/logo_desktop.png" alt="logo" class="logo-desktop">
        <div class="tg-desktop">
            <span>Телеграм-канал</span>
            <img src="@/assets/tg-d.svg" alt="tg">
        </div>
        <TheLoader v-if="isLoading" />
        <div class="mobile-window" v-else>
            <div class="greetings">
                <!-- <TheLoader v-if="isLoading" /> -->
                <img src="@/assets/back.png" alt="back" class="greetings__background">
                <img src="@/assets/hello.png" alt="goose" class="greetings__gosha_begin first-goose">
                <img src="@/assets/open_eyes.png" alt="goose" class="greetings__gosha_after second-goose"
                    ref="secondGoose" v-show="!goose && !smirk">
                <div class="greetings__dialog" v-if="isVisible">
                    <img src="@/assets/dialog.png" alt="dialog" class="greetings__dialog-bubble">
                    <span>Привет! На связи Гоша — всесторонне одарённый инженер.</span>
                </div>
                <div class="greetings__info" v-if="isVisible">
                    <span>Одним крылом он поддерживает высокие нагрузки, другим — здоровую атмосферу в
                        команде.<br>Оптимист,
                        юморист и просто хороший <span class="crossed-text">человек</span> гусь!</span>
                </div>
                <button class="greetings__btn-hello" v-if="isVisible" @mousedown="sayHello" @mouseup="resetStyle"
                    @click="moveGoose" ref="btn">Привет!</button>
                <!-- <img src="@/assets/smirkk.png" alt="goose" class="greetings__goose-smirk" v-show="smirk"> -->
                <img src="@/assets/smirk_new.png" alt="goose" class="greetings__goose-smirk" v-show="smirk">
                <!-- <img src="@/assets/Corobchik.svg" alt="box" class="greetings__corobchik" v-show="corobchik"> -->
                <img src="@/assets/Corobchik_neww.png" alt="box" class="greetings__corobchik" v-show="corobchik">
                <div class="greetings__about-korobchik" v-if="corobchikVisible">
                    <img src="@/assets/dialog2.png" alt="dialog" class="greetings__about-korobchik-bubble">
                    <div class="greetings__about-korobchik-text">
                        <span>А это Коробчик — подарок на день рождения и один из 4 000 000+ ежедневных заказов на
                            Ozon.</span>
                        <span>Над оперативностью доставки работают 6 600+ ИТ-специалистов в сложной системе с <br> 7 000
                            микросервисами.</span>
                    </div>
                </div>
                <button class="greetings__btn-korobchik" v-if="corobchikVisible" @mousedown="sayHello"
                    @mouseup="resetStyle" @click="goToForm" ref="btn">Ого, сколько всего</button>
                <img src="@/assets/open_eyes.png" alt="goose" class="third-goose" v-show="goose" ref="lastGoose">

                <!-- <img src="@/assets/corob-hand.png" alt="box" class="greetings__corobchik-hand" v-show="corobchik_hand"> -->
                <img src="@/assets/сorobchik-hand_new.png" alt="box" class="greetings__corobchik-hand"
                    v-show="corobchik_hand">
                <div class="greetings__gosha-corobchik" v-show="corobchik_hand">
                    <span>С Гошей и Коробчиком познакомились, твоя очередь представиться.</span>
                </div>
                <div class="greetings__form-btn" v-show="corobchik_hand">
                    <button @click="openForm">Заполнить данные</button>
                    <div @click="openFormInfo">
                        <img src="@/assets/in.svg" alt="info">
                    </div>
                </div>
                <div class="greetings__form-bubble" v-show="clickInfo">
                    <img src="@/assets/final-bubble.png" alt="dialog">
                    <span>
                        Мы собираем данные для формирования лидерборда и отправки подарков победителям, другие
                        пользователи
                        увидят только твой ник.
                    </span>
                </div>

                <div class="greetings__order-korobchik" v-show="orderVisible">
                    <img src="@/assets/text2.svg" alt="dialog" class="greetings__order-korobchik-bubble">
                    <span>В этой игре тебе предстоит доставить Коробчика имениннику.</span>
                </div>
                <button class="greetings__btn-order-korobchik" v-show="orderVisible" @mousedown="sayHello"
                    @mouseup="resetStyle" ref="btn" @click="goTo">Не терпится начать!</button>
            </div>
        </div>
    </div>
</template>

<script>
import TheLoader from '@/components/TheLoader.vue';
import axios from 'axios';

export default {
    components: {
        TheLoader
    },
    data() {
        return {
            isLoading: true,
            isVisible: true,
            corobchikVisible: false,
            orderVisible: false,
            corobchik: false,
            goose: false,
            smirk: false,
            corobchik_hand: false,
            clickInfo: false,
        }
    },
    methods: {
        expandTelegramWebApp() {
            if (window.Telegram?.WebApp) {
                const webApp = window.Telegram.WebApp;
                webApp.expand(); // Разворачивает WebApp на весь экран
                console.log("WebApp развернут.");
            } else {
                console.error("Telegram WebApp API недоступен.");
            }
        },
        setCookie(name, value, days) {
            const date = new Date();
            date.setTime(date.getTime() + days * 24 * 60 * 60 * 1000);
            const expires = `expires=${date.toUTCString()}`;
            document.cookie = `${name}=${value};${expires};path=/;SameSite=None;Secure`;
        },
        sayHello() {
            let el = this.$refs.btn;
            el.style.boxShadow = 'none';
        },
        resetStyle() {
            let el = this.$refs.btn;
            setTimeout(() => {
                el.style.boxShadow = '';
            }, 100);
        },
        moveGoose() {
            let goose = this.$refs.secondGoose;
            goose.classList.add('diagonal-move');
            this.isVisible = false;
            goose.addEventListener('transitionend', this.nextStep);
        },
        nextStep() {
            let goose = this.$refs.secondGoose;
            goose.removeEventListener('transitionend', this.nextStep);
            this.corobchikVisible = true;
            this.corobchik = true;
            setTimeout(() => {
                this.smirk = true;
            }, 0);
        },
        goToForm() {
            this.corobchik = false;
            this.corobchik_hand = true;
            this.goose = true;
            this.smirk = false;
            this.corobchikVisible = false;
        },
        openFormInfo() {
            this.clickInfo = !this.clickInfo;
        },
        openForm() {
            this.$router.push('/ozon-form');
        },
        goToNextScreen() {  //change!!!!
            this.orderVisible = true;
            this.corobchik = true;
            this.goose = true;
            this.corobchik_hand = false;
            this.clickInfo = false;

            let goose = this.$refs.lastGoose;
            goose.classList.add('last-goose');
        },
        goTo() {
            this.$router.push('/chat/obx');
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
        getFormResult() {
            const uuid = this.getCookie("uuid");

            if (!uuid) {
                console.error("Куки с именем 'uuid' не найдены.");
                return;
            }

            let url = 'https://ozontechhrbot.ru/api/form/get';

            axios
                .get(url, {
                    params: {
                        "code": uuid,
                    },
                    headers: {
                        "Content-Type": "application/json",
                        "Accept": "application/json",
                    },
                })
                .then(res => {
                    console.log(res.data);
                    if (res.data.result === true) {
                        this.$router.push('/finals/final');
                    }
                })
                .catch(error => {
                    console.error(error);
                })
        },
        preloadImages(imagePaths) {
            return Promise.all(imagePaths.map(src => {
                return new Promise((resolve, reject) => {
                    const img = new Image();
                    img.src = src;
                    img.onload = resolve;
                    img.onerror = reject;
                });
            }));
        }
    },
    mounted() {
        const images = import.meta.glob('@/assets/*.{png,svg}', { eager: true });
        const imageUrls = Object.values(images).map(img => img.default);
        this.expandTelegramWebApp();
        const webApp = window.Telegram?.WebApp;
        const urlParams = new URLSearchParams(window.location.search);
        const uuid = urlParams.get('query');
        // console.log(getCookie('uuid'));
        if (uuid) {
            console.log('Полученный UUID:', uuid);
            this.setCookie('uuid', uuid, 7); // Сохраняем UUID на 7 дней
        } else {
            console.error('UUID отсутствует в параметре query.');
        }
        if (webApp) {
            // Проверяем, доступен ли user
            const user = webApp.initDataUnsafe?.user;

            if (user && user.id) {
                console.log('User ID:', user.id); // Логируем user_id
                this.setCookie('user_id', user.id, 7);
            } else {
                console.error('User ID недоступен.');
            }
        } else {
            console.error('Telegram WebApp API не доступен.');
        }
        this.fetchCsrfToken();


        const formResultPromise = this.getFormResult();


        const imagesPromise = this.preloadImages([
            new URL('@/assets/logo_desktop.png', import.meta.url).href,
            new URL('@/assets/tg-d.svg', import.meta.url).href,
            new URL('@/assets/back.png', import.meta.url).href,
            new URL('@/assets/hello.png', import.meta.url).href,
            new URL('@/assets/open_eyes.png', import.meta.url).href,
            new URL('@/assets/dialog.png', import.meta.url).href,
            new URL('@/assets/dialog2.png', import.meta.url).href,
            new URL('@/assets/smirk_new.png', import.meta.url).href,
            new URL('@/assets/Corobchik_neww.png', import.meta.url).href,
            new URL('@/assets/сorobchik-hand_new.png', import.meta.url).href,
            new URL('@/assets/text2.svg', import.meta.url).href,
            new URL('@/assets/final-bubble.png', import.meta.url).href
        ]);


        Promise.all([formResultPromise, imagesPromise])
            .then(() => {
                setTimeout(() => {
                    this.isLoading = false;
                }, 500);
            })
            .catch(err => {
                console.error("Ошибка при загрузке данных или изображений:", err);
                this.isLoading = false;
            });
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
    overflow: hidden;
    border-radius: 29px 29px 0 0;
    box-shadow: 0 0 14px 0 #032b44;

    @media (max-width: 1024px) {
        width: 390px;
    }

    @media (max-width: 480px) {
        width: 100%;
        border-radius: 0;
    }
}

.greetings {
    position: relative;
    overflow: hidden;
    height: 90vh;

    @media (max-width: 480px) {
        height: 100vh;
    }

    &__background {
        width: 100%;
        height: 90vh;

        @media (max-width: 480px) {
            height: 100vh;
        }

    }

    &__gosha_begin {
        position: absolute;
        left: 50%;
        top: 32.5%;
        right: 0;
        bottom: 0;
        width: 100%;
        transform: translateX(-50%) scale(1);
        transition: transform 3s ease-in-out;

        @media (max-width: 1920px) {
            left: 48%;
            top: 15%;
            right: 0;
            transform: translateX(-50%) scale(1);
        }

        @media (max-width: 1440px) {
            top: 23%;
            right: 0;
            transform: translateX(-50%) scale(1);
        }

        @media (max-width: 800px) {
            top: 20%;
            left: 50%;
            width: -webkit-fill-available;
            transform: translateX(-50%) scale(1);
        }

        @media (max-width: 600px) {
            top: calc(50% - 290px);
            width: 100%;
        }

        @media (max-width: 430px) {
            top: 23%;
            width: 100%;
        }

        @media (max-width: 425px) {
            top: 18%;
        }

        @media (max-width: 420px) {
            top: 23%;
        }

        @media (max-width: 375px) {
            top: 18%;
        }
    }

    &__gosha_after {
        position: absolute;
        left: 50%;
        top: 35%;
        right: 0;
        bottom: 0;
        width: 100%;
        transform: translateX(-47.5%) scale(1.32);
        transition: transform 3s ease-in-out;

        @media (max-width: 1920px) {
            top: 17%;
            transform: translateX(-47.5%) scale(1.2);
        }

        @media (max-width: 1440px) {
            left: 50%;
            top: 26%;
            right: 0;
            bottom: 0;
            transform: translateX(-47.5%) scale(1.32);
        }

        @media (max-width: 800px) {
            top: 23%;
            width: -webkit-fill-available;
        }

        @media (max-width: 600px) {
            top: calc(50% - 270px);
            width: 100%;
        }

        @media (max-width: 430px) {
            transform: translateX(-47.5%) scale(1.34);
            top: 26%;
            width: 100%;
        }

        @media (max-width: 425px) {
            top: 20%;
        }

        @media (max-width: 420px) {
            top: 26%;
        }

        @media (max-width: 390px) {
            transform: translateX(-47.5%) scale(1.32);
            top: 26%;
        }

        @media (max-width: 375px) {
            transform: translateX(-47.5%) scale(1.32);
            top: 22%;
        }
    }

    &__dialog {
        opacity: 0;
        animation: fadeIn 0.8s forwards;
        animation-delay: 6s;

        position: absolute;
        top: 40px;

        @media (max-width: 800px) {
            top: 0;
            left: 200px;
        }

        @media (max-width: 600px) {
            top: 0;
            left: auto;
        }

        @media (max-width: 430px) {
            top: 40px;
        }

        @media (max-width: 425px) {
            top: 0;
        }

        @media (max-width: 420px) {
            top: 40px;
        }

        &-bubble {
            max-width: 100%;

            @media (max-width: 1920px) {
                width: 539px;
            }

            @media (max-width: 800px) {
                width: 450px;
            }

            @media (max-width: 600px) {
                width: auto;
            }

            @media (max-width: 430px) {
                width: 450px;
            }
        }

        span {
            position: absolute;
            top: calc(35% - 140px);
            left: 35px;
            right: 35px;
            font-family: var(--gte);
            font-weight: 400;
            font-size: 20px;
            color: #073049;

            @media (max-width: 1920px) {
                top: 30px;
                font-size: 29px;
            }

            @media (max-width: 1440px) {
                top: 25px;
                font-size: 20px;
            }

            @media (max-width: 800px) {
                top: 30px;
            }

            @media (max-width: 600px) {
                left: 30px;
                right: 30px;
            }

            @media (max-width: 430px) {
                top: 30px;
                left: 35px;
                right: 35px;
            }

            @media (max-width: 420px) {
                top: 25px;
            }

            @media (max-width: 375px) {
                // top: calc(35% - 180px);
                top: 20px;
                left: 30px;
                right: 20px;
            }
        }
    }

    &__info {
        border-radius: 7px;
        padding: 11px;
        backdrop-filter: blur(5px);
        background: rgba(1, 25, 81, 0.85);
        position: absolute;
        bottom: 100px;
        right: 13.5px;
        left: 13.5px;
        opacity: 0;
        animation: fadeIn 1s forwards;
        animation-delay: 6.5s;

        @media (max-width: 430px) {
            bottom: 142px;
        }

        @media (max-width: 425px) {
            bottom: 85px;
        }

        @media (max-width: 420px) {
            bottom: 125px;
        }

        @media (max-width: 390px) {
            bottom: 110px;
        }

        span {
            font-family: var(--gte);
            font-weight: 400;
            font-size: 14px;
            color: #fff;

            @media (max-width: 1920px) {
                font-size: 20px;
            }

            @media (max-width: 1440px) {
                font-size: 14px;
            }
        }

        .crossed-text {
            position: relative;
            display: inline-block;
        }

        .crossed-text::after {
            content: '';
            position: absolute;
            top: 50%;
            left: 0;
            width: 100%;
            height: 1.35px;
            background-color: #f06;
            transform: rotate(-10deg);
        }
    }

    &__btn-hello {
        border-radius: 5px;
        box-shadow: 0 4px 0 0 #054fd4;
        background: #005bff;
        border: none;
        font-family: var(--gte);
        font-weight: 400;
        font-size: 20px;
        text-align: center;
        color: #fff;
        padding: 11px 0;
        position: absolute;
        left: 13.5px;
        right: 13.5px;
        bottom: 30px;
        opacity: 0;
        cursor: pointer;
        animation: fadeIn 1s forwards;
        animation-delay: 6.5s;

        @media (max-width: 1920px) {
            font-size: 29px;
        }

        @media (max-width: 1440px) {
            font-size: 20px;
        }

        @media (max-width: 430px) {
            bottom: 70px;
        }

        @media (max-width: 425px) {
            bottom: 30px;
        }

        @media (max-width: 420px) {
            bottom: 60px;
        }

        @media (max-width: 390px) {
            bottom: 50px;
        }
    }

    &__corobchik {
        position: absolute;
        left: 0;
        top: 55%;
        opacity: 0;
        animation: fadeIn 1s forwards; //here
        animation-delay: 0s;
        width: 219px;

        @media (max-width: 1920px) {
            left: 8%;
            top: 52%;
        }

        @media (max-width: 1440px) {
            left: 0;
            top: 51%;
        }

        @media (max-width: 800px) {
            top: 55%;
            left: 25%;
        }

        @media (max-width: 600px) {
            left: -4%;
            top: 50%;
        }

        @media (max-width: 430px) {
            top: 50%;
        }

        @media (max-width: 420px) {
            top: 49%;
        }

        @media (max-width: 390px) {
            top: 51%;
            left: -10px;
        }

        @media (max-width: 375px) {
            left: -15px;
        }
    }

    &__about-korobchik {
        opacity: 0;
        animation: fadeIn 1s forwards;
        animation-delay: 1s;

        position: absolute;
        top: 20px;
        right: -10px;


        @media (max-width: 1920px) {
            right: 30px;
            top: 70px;
        }

        @media (max-width: 1440px) {
            right: 0;
            top: 20px;
        }

        @media (max-width: 800px) {
            right: 150px;
            top: 0;
        }

        @media (max-width: 600px) {
            top: 30px;
            right: -10px;
        }

        @media (max-width: 425px) {
            top: 0;
        }

        @media (max-width: 420px) {
            top: 20px;
        }

        @media (max-width: 375px) {
            top: 0;
        }

        &-text {
            display: flex;
            flex-direction: column;
            gap: 3px;
            position: absolute;
            top: 0;
            left: 0;
            right: 0;

            @media (max-width: 1920px) {
                top: 20px;
                left: 20px;
                right: 20px;
            }

            @media (max-width: 800px) {
                top: 20px;
                left: 20px;
                right: 20px;
            }

            span {
                font-family: var(--gte);
                font-weight: 400;
                font-size: 15px;
                color: #073049;
            }
        }
    }

    &__btn-korobchik {
        border-radius: 5px;
        box-shadow: 0 4px 0 0 #054fd4;
        background: #005bff;
        font-family: var(--gte);
        font-weight: 400;
        font-size: 20px;
        text-align: center;
        color: #fff;
        border: none;
        padding: 11px 95px;
        position: absolute;
        bottom: 30px;
        right: 13px;
        left: 13px;
        white-space: nowrap;
        cursor: pointer;
        opacity: 0;
        animation: fadeIn 1s forwards;
        animation-delay: 1s;

        @media (max-width: 1920px) {
            font-size: 29px;
        }

        @media (max-width: 1440px) {
            font-size: 20px;
        }

        @media (max-width: 430px) {
            bottom: 50px;
        }

        @media (max-width: 425px) {
            bottom: 30px;
        }

        @media (max-width: 420px) {
            bottom: 50px;
        }

        @media (max-width: 390px) {
            bottom: 30px;
        }
    }

    &__order-korobchik {
        opacity: 0;
        animation: fadeIn 0.5s forwards;

        &-bubble {
            position: absolute;
            top: 160px;
            right: 0;

            @media (max-width: 800px) {
                top: 20px;
                right: 170px;
            }

            @media (max-width: 430px) {
                top: 120px;
                right: 20px;
            }

            @media (max-width: 420px) {
                top: 100px;
                right: 20px;
            }

            @media (max-width: 390px) {
                top: 90px;
                right: 5px;
            }

            @media (max-width: 375px) {
                top: 50px;
            }
        }

        span {
            font-family: var(--gte);
            font-weight: 400;
            font-size: 16px;
            color: #073049;
            position: absolute;
            top: 180px;
            right: 30px;
            left: 100px;

            @media (max-width: 800px) {
                top: 40px;
                left: 350px;
                right: 200px;
            }

            @media (max-width: 430px) {
                top: 140px;
                left: 115px;
                right: 30px;
            }

            @media (max-width: 420px) {
                top: 120px;
                left: 97px;
            }

            @media (max-width: 390px) {
                top: 110px;
                left: 90px;
                right: 20px;
            }

            @media (max-width: 375px) {
                top: 70px;
                right: 40px;
                left: 80px;
            }
        }
    }

    &__btn-order-korobchik {
        border-radius: 5px;
        box-shadow: 0 4px 0 0 #054fd4;
        background: #005bff;
        font-family: var(--gte);
        font-weight: 400;
        font-size: 20px;
        text-align: center;
        color: #fff;
        border: none;
        padding: 11px 87px;
        position: absolute;
        bottom: 30px;
        right: 13px;
        left: 13px;
        cursor: pointer;
        opacity: 0;
        white-space: nowrap;
        animation: fadeIn 0.5s forwards;

        @media (max-width: 430px) {
            bottom: 50px;
        }

        @media (max-width: 420px) {
            bottom: 50px;
        }

        @media (max-width: 390px) {
            bottom: 30px;
        }
    }

    &__corobchik-hand {
        position: absolute;
        left: 0;
        top: 55%;
        opacity: 0;
        // animation: fadeIn 0.5s forwards;
        animation: fadeIn 0.5s ease-in-out forwards;
        animation-delay: 0s;
        width: 219px;

        @media (max-width: 1920px) {
            top: 52%;
            left: 7%;
        }

        @media (max-width: 1440px) {
            top: 52%;
            left: -10px;
        }

        @media (max-width: 800px) {
            top: 55%;
            left: 25%;
        }

        @media (max-width: 600px) {
            left: 0;
            top: 50%;
        }

        @media (max-width: 430px) {
            top: 50%;
            left: -4%;
        }

        @media (max-width: 420px) {
            top: 49%;
        }

        @media (max-width: 390px) {
            top: 51%;
        }
    }


    &__gosha-corobchik {
        border-radius: 7px;
        padding: 10px;
        backdrop-filter: blur(6.741573333740234px);
        background: rgba(1, 25, 81, 0.85);
        position: absolute;
        top: 20px;
        left: 13px;
        right: 13px;

        span {
            font-family: var(--gte);
            font-weight: 400;
            font-size: 19px;
            color: #fff;
        }
    }

    &__form-btn {
        display: flex;
        justify-content: center;
        align-items: center;
        gap: 13px;
        position: absolute;
        bottom: 30px;
        right: 13px;
        left: 13px;

        @media (max-width: 420px) {
            bottom: 50px;
        }

        @media (max-width: 390px) {
            bottom: 30px;
        }

        button {
            border-radius: 5px;
            border: none;
            box-shadow: 0 4px 0 0 #054fd4;
            background: #005bff;
            font-family: var(--gte);
            font-weight: 400;
            font-size: 20px;
            text-align: center;
            color: #fff;
            padding: 11px 63px;
            white-space: nowrap;
            width: 100%;
            cursor: pointer;

            @media (max-width: 1920px) {
                font-size: 29px;
            }

            @media (max-width: 1440px) {
                font-size: 20px;
            }
        }

        div {
            border-radius: 5px;
            padding: 9px;
            box-shadow: 0 4px 0 0 #054fd4;
            background: #005bff;

            @media (max-width: 1920px) {
                padding: 15px;
            }

            @media (max-width: 1440px) {
                padding: 11px 11px 11px 13px;
            }

            @media (max-width: 800px) {
                padding: 9px;
            }
        }
    }

    &__form-bubble {
        position: absolute;
        bottom: 70px;
        right: 0;

        @media (max-width: 420px) {
            bottom: 80px;
        }

        @media (max-width: 390px) {
            bottom: 70px;
        }

        span {
            position: absolute;
            top: 20px;
            left: 80px;
            right: 20px;
            font-family: var(--gte);
            font-weight: 400;
            font-size: 14px;
            color: #073049;

            @media (max-width: 1920px) {
                left: 20px;
            }

            @media (max-width: 800px) {
                left: 20px;
            }
        }
    }

    &__goose-smirk {
        position: absolute;
        top: 55%;
        right: 0;
        left: 22%;
        opacity: 0;
        animation: fadeIn 1s forwards;
        animation-delay: 0s;
        width: 334px;

        @media (max-width: 1920px) {
            top: 21%;
            left: 37%;
        }

        @media (max-width: 1024px) {
            top: 15%;
            left: 35%;
        }

        @media (max-width: 800px) {
            left: 46%;
            // top: 21%;
            top: calc(40% - 180px);
        }

        @media (max-width: 600px) {
            top: calc(42% - 200px);
            left: auto;
            right: -80px;
        }

        @media (max-width: 430px) {
            left: 37%;
            top: calc(42% - 200px);
            right: 0;
        }

        @media (max-width: 425px) {
            top: calc(40% - 200px);
        }

        @media (max-width: 420px) {
            left: 35%;
            top: calc(42% - 210px);
            // top: 25%;
        }

        @media (max-width: 390px) {
            top: calc(43% - 210px);
        }

        @media (max-width: 375px) {
            left: 38%;
            top: calc(50% - 210px);
            width: 300px;
        }
    }

    .last-goose {
        opacity: 0;
        animation: fadeOut 0.2s forwards, fadeIn 0.1s forwards;
    }

    @keyframes zoomIn {
        0% {
            transform: translateX(-50%) scale(1);
        }

        50% {
            transform: translateX(-50%) scale(1);
        }

        100% {
            transform: translateX(-50%) scale(1.2);
        }
    }

    @keyframes fadeOut {
        to {
            opacity: 0;
        }
    }

    @keyframes fadeIn {
        to {
            opacity: 1;
        }
    }

    @keyframes fadeInNew {
        from {
            opacity: 0;
        }

        to {
            opacity: 1;
        }
    }
}

.first-goose {
    animation: zoomIn 2s forwards, fadeOut 1s forwards;
    // animation: zoomIn 2s forwards;
    animation-delay: 1s, 4s;
    transform-origin: center;
}

.second-goose {
    opacity: 0;
    animation: fadeIn 1s forwards;
    animation-delay: 4s;
}

.third-goose {
    opacity: 0;
    transition: opacity 0.5s ease-in-out;
    position: absolute;
    left: 22%;
    top: 27%;
    width: 360px;
    animation: fadeIn 0.5s forwards; //here

    @media (max-width: 1920px) {
        top: 21%;
        left: 35%;
    }

    @media (max-width: 1440px) {
        top: 15%;
        left: 33%;
    }

    @media (max-width: 800px) {
        top: calc(40% - 180px);
        left: 46%;
    }

    @media (max-width: 600px) {
        top: calc(50% - 260px);
        left: auto;
        right: -80px;
    }

    @media (max-width: 430px) {
        top: calc(42% - 200px);
        left: 35%;
    }

    @media (max-width: 425px) {
        top: calc(40% - 200px);
    }

    @media (max-width: 420px) {
        top: calc(42% - 210px);
    }

    @media (max-width: 390px) {
        top: calc(43% - 210px);
        left: 32%;
    }

    @media (max-width: 375px) {
        top: 17%;
        left: 36%;
        width: 330px;
    }
}

.diagonal-move {
    transform: translate(-22.5%, -27%) scale(0.9);
    opacity: 1;
    animation-duration: 1s;

    @media (max-width: 1920px) {
        transform: translate(-34%, -9%) scale(0.7);
    }

    @media (max-width: 1440px) {
        transform: translate(-22%, -15%) scale(0.85);
    }

    @media (max-width: 800px) {
        transform: translate(-14%, -18%) scale(0.9);
    }

    @media (max-width: 600px) {
        transform: translate(-19%, -10%) scale(0.8);
    }

    @media (max-width: 430px) {
        transform: translate(-24%, -18%) scale(0.8);
    }

    @media (max-width: 420px) {
        transform: translate(-24%, -18%) scale(0.83);
    }

    @media (max-width: 390px) {
        transform: translate(-22%, -18%) scale(0.86);
    }

    @media (max-width: 375px) {
        transform: translate(-22%, -12%) scale(0.85);
    }
}
</style>
