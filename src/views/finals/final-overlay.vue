<template>
    <div class="desktop-background">
        <img src="@/assets/logo_desktop.png" alt="logo" class="logo-desktop">
        <div class="tg-desktop" @click="openTelegram">
            <span>Телеграм-канал</span>
            <img src="@/assets/tg-d.svg" alt="tg">
        </div>
        <div class="mobile-window">
            <div class="page">
                <div class="header">
                    <img src="@/assets/logo2.svg" alt="logo">
                    <div class="header__menu">
                        <img src="@/assets/hamb.svg" alt="menu">
                        <span>Меню</span>
                    </div>
                </div>

                <div class="header__menu_wrap" v-show="menu">
                    <div class="header__menu_overlay" @click="openMenu">
                        <img src="@/assets/hamb.svg" alt="menu">
                        <span>Меню</span>
                    </div>
                </div>

                <img src="@/assets/strelka.svg" alt="arrow" class="arrow" v-show="showArrow">

                <div class="text" v-show="showText">
                    <span>
                        Нажми сюда, чтобы запустить игру заново, посмотреть таблицу лидеров или открыть телеграм-канал
                        Ozon
                        Tech.
                    </span>
                </div>

                <!-- <div class="tap" v-show="showTap" @click="goToFinal">
            <img src="@/assets/tap.svg" alt="tap">
        </div> -->

                <div class="tap-container" v-show="showTap" @click="goToFinal">
                    <img src="@/assets/tap.svg" alt="tap" class="tap">
                </div>


                <img src="@/assets/hello.png" alt="logo" class="goose">
                <div class="button">
                    <span>Вакансии в Ozon Tech</span>
                    <img src="@/assets/op.svg" alt="go">
                </div>
            </div>

            <TheMenu v-if="showMenu" @close="closeMenu">
                <template #title></template>
                <template #body>
                    <div class="menu">
                        <div @click="openChat">
                            <img src="@/assets/reload.svg" alt="reload">
                            <span>Сыграть ещё раз</span>
                        </div>
                        <div @click="openLeaderboard">
                            <img src="@/assets/coin.svg" alt="coin">
                            <span>Мои баллы</span>
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
            menu: true,
            showText: false,
            showArrow: false,
            showTap: false,
            showMenu: false,
        };
    },
    mounted() {
        setTimeout(() => {
            this.showArrow = true;
        }, 800);
        setTimeout(() => {
            this.showText = true;
        }, 1600);
        setTimeout(() => {
            this.showTap = true;
        }, 2000);
    },
    methods: {
        openTelegram() {
            window.open('https://t.me/s/ozon_tech', '_blank', 'noopener,noreferrer');
        },
        goToFinal() {
            this.$router.push('/finals/final');
        },
        openMenu() {
            this.showTap = false;
            this.showArrow = false;
            this.showText = false;
            this.menu = false;
            this.showMenu = true;
            document.body.style.overflow = 'hidden';
        },
        openLeaderboard() {
            this.$router.push('/finals/leaderboard');
            document.body.style.overflow = '';
        },
        closeMenu() {
            this.showMenu = false;
            this.$router.push('/finals/final');
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

    &::before {
        content: "";
        position: fixed;
        top: 90px;
        // left: 690px;
        width: 539px;
        height: 90vh;
        background-color: rgba(0, 0, 0, 0.9);
        z-index: 2;
        border-radius: 29px 29px 0 0;

        @media (max-width: 1440px) {
            top: 80px;
            left: 525px;
            width: 390px;
        }

        @media (max-width: 480px) {
            width: 100%;
            height: 100dvh;
            top: 0;
            left: 0;
            border-radius: 0;
        }
    }
}

.page {
    overflow: hidden;
    position: relative;
    height: 100dvh;
    background-color: #02283E;
    background-image: url(@/assets/Vector.png);
    background-size: cover;
    padding: 0 17px;

    @media (max-width: 1920px) {
        height: 90vh;
    }

    @media (max-width: 800px) {
        height: 100dvh;
    }

    &::before {
        content: "";
        position: fixed;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        background-color: rgba(0, 0, 0, 0.9);
        z-index: 2;

        @media (max-width: 1920px) {
            display: none;
        }
    }

    .header {
        z-index: 1;

        &__menu {
            z-index: 10;
            pointer-events: all;
        }
    }
}

.header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    border-bottom: 0.67px solid rgba(255, 255, 255, 0.2);
    border-radius: 0 0 13px 13px;
    background: #0a3956;
    z-index: 10000;
    position: fixed;
    top: 0;
    left: 0;
    right: 0;

    @media (max-width: 1920px) {
        position: static;
    }

    @media (max-width: 800px) {
        position: fixed;
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
        left: 150px;
    }

    @media (max-width: 1440px) {
        left: 80px;
    }

    @media (max-width: 800px) {
        left: 300px;
        width: 238px;
    }

    @media (max-width: 600px) {
        left: 130px;
        width: 254px;
    }

    @media (max-width: 430px) {
        left: 80px;
        width: 254px;
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

.header__menu_wrap {
    border: 0.67px solid #005bff;
    border-radius: 27px;
    position: absolute;
    z-index: 100;
    top: 15px;
    right: 8px;
    padding: 13px 12px 13px 11px;
    background: #0a3956;

    @media (max-width: 1920px) {
        top: 5px;
        right: 20px;
    }

    @media (max-width: 800px) {
        top: 15px;
        right: 8px;
    }
}

.header__menu_overlay {
    border-radius: 25px;
    padding: 6px 12px;
    height: 33px;
    background: #005bff;
    display: flex;
    gap: 7px;
    align-items: center;

    span {
        font-family: var(--gte);
        font-weight: 400;
        font-size: 16px;
        color: #fff;
    }
}

.arrow {
    position: absolute;
    z-index: 100;
    top: 80px;
    right: 130px;
}

.text {
    border-radius: 7px;
    padding: 10px 13px;
    backdrop-filter: blur(6.741573333740234px);
    background: #fff;
    text-align: center;
    position: absolute;
    top: 150px;
    left: 28px;
    right: 28px;
    z-index: 100;

    span {
        font-family: var(--gte);
        font-weight: 400;
        font-size: 14px;
        line-height: 120%;
        text-align: center;
        color: #073049;
    }
}

.tap {
    position: absolute;
    z-index: 100;
    bottom: 50px;
    right: 18px;
    animation: heartbeat 1s infinite;
    cursor: pointer;

    @media (max-width: 1920px) {
        right: 730px;
    }

    @media (max-width: 1440px) {
        right: 550px;
    }

    @media (max-width: 800px) {
        right: 18px;
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
</style>
