<template>
    <div class="desktop-background">
        <img src="@/assets/logo_desktop.png" alt="logo" class="logo-desktop">
        <div class="tg-desktop" @click="openTelegram">
            <span>Телеграм-канал</span>
            <img src="@/assets/tg-d.svg" alt="tg">
        </div>
        <div class="mobile-window">
            <div class="greetings">
                <img src="@/assets/back.png" alt="back" class="greetings__background">
                <img src="@/assets/open_eyes.png" alt="goose" class="greetings__goose">
                <img src="@/assets/Corobchik_neww.png" alt="box" class="greetings__corobchik">
                <div class="greetings__dialog">
                    <img src="@/assets/dialog3.png" alt="dialog">
                    <span>В этой игре тебе предстоит доставить Коробчика имениннику.</span>
                </div>
                <button class="greetings__button" @mousedown="sayHello" @mouseup="resetStyle" ref="btn" @click="goTo">Не
                    терпится начать!</button>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    methods: {
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
        goTo() {
            this.$router.push('/chat/obx');
        },
        openTelegram() {
            window.open('https://t.me/s/ozon_tech', '_blank', 'noopener,noreferrer');
        }
    },
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

    @media (max-width: 1440px) {
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
        height: auto;
    }

    &__background {
        width: 100%;
        height: 90vh;

        @media (max-width: 480px) {
            height: 100dvh;
        }
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
        padding: 11px 87px;
        position: absolute;
        bottom: 30px;
        right: 13px;
        left: 13px;
        cursor: pointer;
        opacity: 0;
        white-space: nowrap;
        animation: fadeIn 0.5s forwards;

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

    &__goose {
        position: absolute;
        left: 22%;
        top: 27%;
        width: 336px;
        opacity: 0;
        animation: fadeIn 1s forwards;

        @media (max-width: 1920px) {
            top: 24%;
            left: 35%;
        }

        @media (max-width: 1440px) {
            top: 17%;
        }

        @media (max-width: 800px) {
            top: 14%;
            left: 46%;
            width: 350px;
        }

        @media (max-width: 600px) {
            top: calc(50% - 260px);
            right: -80px;
            left: auto;
        }

        @media (max-width: 430px) {
            right: auto;
            top: calc(45% - 200px);
            left: 40%;
            width: 336px;
        }

        @media (max-width: 425px) {
            top: calc(44% - 200px);
        }

        @media (max-width: 420px) {
            top: calc(45% - 210px);
            left: 37%;
        }

        @media (max-width: 390px) {
            top: calc(47% - 210px);
            left: 35%;
        }

        @media (max-width: 375px) {
            top: calc(48% - 210px);
            left: 36%;
        }
    }

    &__corobchik {
        position: absolute;
        left: 0;
        top: 55%;
        opacity: 0;
        animation: fadeIn 1s forwards;
        width: 219px;

        @media (max-width: 1920px) {
            top: 52%;
            left: 8%;
        }

        @media (max-width: 1440px) {
            top: 50%;
            left: -3%;
        }

        @media (max-width: 800px) {
            top: 55%;
            left: 25%;
        }

        @media (max-width: 600px) {
            left: -10px;
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
        }
    }

    &__dialog {
        position: absolute;
        top: 20px;
        right: 0;
        opacity: 0;
        animation: fadeIn 1s forwards;

        @media (max-width: 1920px) {
            top: 170px;
            right: 90px;
        }

        @media (max-width: 1440px) {
            top: 70px;
            right: 0;
        }

        @media (max-width: 800px) {
            top: 20px;
            right: 180px;
        }

        @media (max-width: 600px) {
            top: 80px;
            right: 0;
        }

        @media (max-width: 430px) {
            top: 110px;
        }

        @media (max-width: 425px) {
            top: 30px;
        }

        @media (max-width: 420px) {
            top: 80px;
        }

        @media (max-width: 375px) {
            top: 50px;
        }

        span {
            font-family: var(--gte);
            font-weight: 400;
            font-size: 16px;
            color: #073049;
            position: absolute;
            top: 20px;
            right: 0;
            left: 20px;
            opacity: 0;
            animation: fadeIn 2s forwards;
        }
    }
}

@keyframes fadeIn {
    to {
        opacity: 1;
    }
}
</style>