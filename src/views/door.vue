<template>
    <div class="desktop-background">
        <img src="@/assets/logo_desktop.png" alt="logo" class="logo-desktop">
        <div class="tg-desktop">
            <span>Телеграм-канал</span>
            <img src="@/assets/tg-d.svg" alt="tg">
        </div>
        <TheLoader v-if="isLoading" />
        <div class="mobile-window" v-else>

            <div class="page">
                <img src="@/assets/door_back.png" alt="background" class="background" v-show="showBackground">
                <div class="door" ref="door">
                    <!-- <TheLoader v-if="isLoading" /> -->
                    <img src="@/assets/door.png" alt="door" class="door-back" v-show="showDoor">
                    <img src="@/assets/open_eyes.png" alt="together" class="door-gosha" v-show="showCharacters">
                    <img src="@/assets/Corobchik.png" alt="box" class="door-corobchik" v-show="showCharacters">
                    <div class="door__dialog" v-if="showMsg">
                        <img src="@/assets/fignya1.png" alt="text" class="door__dialog-bubble">
                        <span>О, мы как раз вовремя!</span>
                    </div>
                    <Transition name="fade">
                        <img src="@/assets/flag.png" alt="flag" v-show="showFlag" class="flag">
                    </Transition>
                    <img src="@/assets/all_new.png" alt="together" v-show="showFinalCharacters"
                        class="final-characters">
                    <div class="lizard__dialog" v-show="showMsg2">
                        <img src="@/assets/place.png" alt="text" class="lizard__dialog-bubble">
                        <span>Ура, спасибо за подарки!</span>
                    </div>

                    <div class="points" v-show="showPoints">
                        <p>Набранные баллы:</p>
                        <span>1142</span>
                    </div>

                    <div v-show="showBtn">
                        <button @click="goToPreFinal">Было интересно!</button>
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
            showCharacters: false,
            showMsg: false,
            showDoor: true,
            showFlag: false,
            showFinalCharacters: false,
            showMsg2: false,
            showPoints: false,
            showBtn: false,
            showBackground: false,
        }
    },
    mounted() {
        setTimeout(() => {
            this.showDoor = true;
            this.showBackground = true;
        }, 0);
        this.preloadImages([
            new URL('@/assets/logo_desktop.png', import.meta.url).href,
            new URL('@/assets/tg-d.svg', import.meta.url).href,
            new URL('@/assets/door_back.png', import.meta.url).href,
            new URL('@/assets/door.png', import.meta.url).href,
            new URL('@/assets/open_eyes.png', import.meta.url).href,
            new URL('@/assets/Corobchik.png', import.meta.url).href,
            new URL('@/assets/fignya1.png', import.meta.url).href,
            new URL('@/assets/flag.png', import.meta.url).href,
            new URL('@/assets/all_new.png', import.meta.url).href,
            new URL('@/assets/place.png', import.meta.url).href
        ]).then(() => {
            this.isLoading = false; // Убираем лоадер после загрузки всех изображений
            // this.startAnimation(); // Запускаем анимации
        }).catch(err => {
            console.error("Ошибка загрузки изображений:", err);
            this.isLoading = false; // В любом случае скрываем лоадер
        });

        setTimeout(() => {
            this.showCharacters = true;
            // this.isLoading = false;
        }, 1000);

        setTimeout(() => {
            this.showMsg = true;
        }, 3000);

        setTimeout(() => {
            this.showMsg = false;
            this.showCharacters = false;
            this.showDoor = false;
            this.showFlag = true;
        }, 4000);
        setTimeout(() => {
            this.showFinalCharacters = true;
        }, 5000);
        setTimeout(() => {
            this.showMsg2 = true;
        }, 6000);
        setTimeout(() => {
            this.showPoints = true;
        }, 7000);
        setTimeout(() => {
            this.showBtn = true;
        }, 8000);
    },
    methods: {
        goToPreFinal() {
            this.$router.push('/pre-final');
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
    }
}
</script>

<style lang="scss">
.fade-enter-active,
.fade-leave-active {
    transition: opacity 0.5s ease;
}

.fade-enter-from,
.fade-leave-to {
    opacity: 0;
}

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
        box-shadow: none;
    }
}

.page {
    position: relative;
    overflow: hidden;
}

.background {
    pointer-events: none;
    position: absolute;
    width: 100%;
    height: 100%;
    z-index: -1;

    @media (max-width: 1920px) {
        z-index: 1;
    }

    @media (max-width: 800px) {
        z-index: -1;
    }
}

.door {
    width: 100%;
    height: 100vh;
    // background: url(@/assets/door_back.png) no-repeat center fixed;
    background-size: cover;
    background-position: center;
    background-attachment: fixed;

    @media (max-width: 1920px) {
        height: 90vh;
    }

    @media (max-width: 800px) {
        height: 100vh;
    }

    &-back {
        position: absolute;
        bottom: 0;
        width: 100%;
        z-index: 1;

        @media (max-width: 1920px) {
            height: 780px;
        }

        @media (max-width: 1440px) {
            left: 0;
            height: auto;
            width: 100%;
        }

        @media (max-width: 800px) {
            height: -webkit-fill-available;
            width: auto;
            left: 30%;
        }

        @media (max-width: 600px) {
            left: 0;
            height: auto;
            width: 100%;
        }

        @media (max-width: 430px) {
            left: 0;
            height: auto;
            width: 100%;
        }

        @media (max-width: 425px) {
            bottom: auto;
        }

        @media (max-width: 420px) {
            bottom: 0;
        }
    }

    &-gosha {
        position: absolute;
        bottom: -70px;
        left: -100px;
        z-index: 100;
        width: 100%;
        animation: fadeIn 1s forwards;
        opacity: 0;

        @media (max-width: 1920px) {
            bottom: -100px;
            left: -120px;
        }

        @media (max-width: 1440px) {
            bottom: -70px;
            left: -100px;
        }

        @media (max-width: 800px) {
            width: 350px;
            left: 20%;
        }

        @media (max-width: 600px) {
            width: 100%;
            left: -100px;
        }

        @media (max-width: 430px) {
            width: 100%;
            left: -100px;
        }
    }

    &-corobchik {
        position: absolute;
        bottom: -50px;
        right: -30px;
        z-index: 100;
        width: 262px;
        animation: fadeIn 1s forwards;
        opacity: 0;

        @media (max-width: 1920px) {
            width: 320px;
        }

        @media (max-width: 1440px) {
            width: 262px;
        }

        @media (max-width: 800px) {
            width: 250px;
            left: 48%;
        }

        @media (max-width: 600px) {
            width: 280px;
            left: auto;
            right: -20px;
        }

        @media (max-width: 430px) {
            width: 262px;
            left: 40%;
            right: -30px;
        }
    }

    &__dialog {
        position: relative;

        &-bubble {
            position: absolute;
            // width: 250px;
            top: 250px;
            left: 90px;
            z-index: 100;

            @media (max-width: 1920px) {
                top: 200px;
                left: 150px;
            }

            @media (max-width: 1440px) {
                top: 200px;
                left: 90px;
            }

            @media (max-width: 800px) {
                top: 100px;
                left: 300px;
            }

            @media (max-width: 600px) {
                top: 220px;
                left: 130px;
            }

            @media (max-width: 430px) {
                top: 240px;
                left: 90px;
            }

            @media (max-width: 425px) {
                top: 100px;
            }

            @media (max-width: 420px) {
                top: 250px;
            }

            @media (max-width: 390px) {
                top: 200px;
            }

            @media (max-width: 375px) {
                top: 170px;
                left: 80px;
            }
        }

        span {
            font-family: var(--gte);
            font-weight: 400;
            font-size: 20px;
            color: #073049;
            position: absolute;
            top: 270px;
            left: 115px;
            z-index: 101;

            @media (max-width: 1920px) {
                top: 225px;
                left: 175px;
            }

            @media (max-width: 1440px) {
                top: 220px;
                left: 115px;
            }

            @media (max-width: 800px) {
                top: 120px;
                left: 320px;
            }

            @media (max-width: 600px) {
                top: 240px;
                left: 150px;
            }

            @media (max-width: 430px) {
                top: 260px;
                left: 115px;
            }

            @media (max-width: 425px) {
                top: 120px;
            }

            @media (max-width: 420px) {
                top: 270px;
            }

            @media (max-width: 390px) {
                top: 220px;
            }

            @media (max-width: 375px) {
                top: 190px;
                left: 100px;
            }
        }
    }

    img {
        &.flag {
            width: 100%;

            @media (max-width: 1920px) {
                z-index: 100;
                position: absolute;
            }

            @media (max-width: 800px) {
                height: 427px;
                position: static;
            }

            @media (max-width: 430px) {
                height: auto;
            }

            @media (max-width: 425px) {
                height: 427px;
            }

            @media (max-width: 420px) {
                height: auto;
            }
        }
    }

    img {
        &.final-characters {
            position: absolute;
            bottom: -100%;
            left: 0;
            width: 575px;
            opacity: 0;
            z-index: 100;
            animation: slideUp 1s forwards;

            @media (max-width: 1920px) {
                width: 650px;
                left: -80px;
                top: 380px;
            }

            @media (max-width: 1440px) {
                width: 575px;
                left: -100px;
                top: 300px;
            }

            @media (max-width: 800px) {
                width: 500px;
                left: 150px;
                top: 230px;
            }

            @media (max-width: 600px) {
                width: 575px;
                left: -60px;
                top: 420px;
            }

            @media (max-width: 430px) {
                width: 575px;
                left: -100px;
                top: 400px;
            }

            @media (max-width: 425px) {
                top: 215px;
            }

            @media (max-width: 420px) {
                width: 575px;
                left: -100px;
                top: 350px;
            }

            @media (max-width: 390px) {
                width: 575px;
                left: -100px;
                top: 300px;
            }
        }
    }

    .lizard__dialog {
        position: relative;

        &-bubble {
            position: absolute;
            top: -80px;

            @media (max-width: 1920px) {
                top: 450px;
                left: 50px;
                z-index: 100;
            }

            @media (max-width: 1440px) {
                top: -80px;
                left: 0;
            }

            @media (max-width: 800px) {
                top: -160px;
                left: 210px;
            }

            @media (max-width: 600px) {
                top: 50px;
                left: 30px;
            }

            @media (max-width: 430px) {
                top: -20px;
                left: 0;
            }

            @media (max-width: 425px) {
                top: -160px;
            }

            @media (max-width: 420px) {
                top: -50px;
            }

            @media (max-width: 390px) {
                top: -80px;
            }

            @media (max-width: 375px) {
                top: -110px;
                left: -3px;
            }
        }

        span {
            font-family: var(--gte);
            font-weight: 400;
            font-size: 13px;
            line-height: 120%;
            color: #000;
            position: absolute;
            top: -60px;
            left: 15px;

            @media (max-width: 1920px) {
                top: 470px;
                left: 65px;
                z-index: 100;
            }

            @media (max-width: 1440px) {
                top: -60px;
                left: 20px;
            }

            @media (max-width: 600px) {
                top: 70px;
                left: 45px;
            }

            @media (max-width: 430px) {
                top: 0;
                left: 15px;
            }

            @media (max-width: 425px) {
                top: -140px;
            }

            @media (max-width: 420px) {
                top: -30px;
            }

            @media (max-width: 390px) {
                top: -60px;
            }

            @media (max-width: 375px) {
                top: -90px;
            }
        }
    }

    .points {
        border-radius: 7px;
        padding: 10px 50px;
        backdrop-filter: blur(6.741573333740234px);
        background: #fff;
        margin: 0 88px;
        position: absolute;
        top: 160px;
        text-align: center;
        opacity: 0;
        animation: slideDown 0.8s forwards;

        @media (max-width: 1920px) {
            top: 160px;
            left: 15%;
            z-index: 100;
        }

        @media (max-width: 1440px) {
            left: 1%;
        }

        @media (max-width: 800px) {
            animation: slideDownBigScreen 0.8s forwards;
            left: 200px;
        }

        @media (max-width: 600px) {
            animation: slideDown 0.8s forwards;
            left: 50px;
        }

        @media (max-width: 430px) {
            left: 0;
        }

        @media (max-width: 425px) {
            animation: slideDownDesktop 0.8s forwards;
        }

        @media (max-width: 420px) {
            animation: slideDown 0.8s forwards;
        }

        p {
            margin: 0 0 3px 0;
            font-family: var(--gte);
            font-weight: 400;
            font-size: 14px;
            text-align: center;
            color: #073049;
            white-space: nowrap;
        }

        span {
            font-family: var(--gte);
            font-weight: 500;
            font-size: 43px;
            line-height: 100%;
            text-align: center;
            color: #3adf68;
        }
    }

    button {
        font-family: var(--gte);
        font-weight: 400;
        font-size: 20px;
        text-align: center;
        color: #fff;
        border-radius: 5px;
        box-shadow: 0 4px 0 0 #054fd4;
        background: #005bff;
        border: none;
        padding: 11px 101px;
        position: absolute;
        bottom: 35px;
        left: 10px;
        right: 10px;
        z-index: 1000;
        opacity: 0;
        animation: slideUpBtn 0.5s ease forwards;
    }
}

@keyframes slideUp {
    0% {
        bottom: -100%;
        opacity: 0;
    }

    100% {
        bottom: 0;
        opacity: 1;
    }
}

@keyframes slideUpBtn {
    0% {
        bottom: -100%;
        opacity: 0;
    }

    100% {
        bottom: 40px;
        opacity: 1;
    }
}

@keyframes slideDown {
    0% {
        top: -100%;
        opacity: 0;
    }

    100% {
        top: 160px;
        opacity: 1;
    }
}

@keyframes slideDownBigScreen {
    0% {
        top: -100%;
        opacity: 0;
    }

    100% {
        top: 120px;
        opacity: 1;
    }
}

@keyframes slideDownDesktop {
    0% {
        top: -100%;
        opacity: 0;
    }

    100% {
        top: 120px;
        opacity: 1;
    }
}

@keyframes fadeIn {
    to {
        opacity: 1;
    }
}
</style>
