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
                <div ref="chat" class="chat">
                    <div class="chat__right">
                        <div class="chat__right-bubble right-text-1">
                            <TheChatWhite>
                                <template #chattng__text>
                                    <img src="@/assets/ded.png" alt="pic">
                                    <span>Ребята из Ozon Tech учитывают такие кейсы при расчёте времени доставки,<br> но
                                        мы очень
                                        торопимся.</span>
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
                        <div class="chat__left-avatar left-avatar-1">
                            <img src="@/assets/ava_g.png" alt="avatar" class="avatars">
                        </div>
                        <div class="chat__left-bubble">
                            <TheChatBlue :isLast="true">
                                <template #chattng__text>
                                    <div class="chat__left-bubble_inner">
                                        <span>Коробчик, без паники, давай как на третьем подряд созвоне.</span>
                                    </div>
                                </template>
                            </TheChatBlue>
                        </div>
                    </div>

                    <div class="chat__right_2 show-right-1" v-show="showRight">
                        <div class="chat__right-bubble">
                            <TheChatWhite>
                                <template #chattng__text>
                                    <span>Включаю режим созвоноустойчивости.</span>
                                </template>
                            </TheChatWhite>
                        </div>
                        <div class="chat__right-avatar">
                            <img src="@/assets/ava_b.png" alt="avatar" class="avatars">
                        </div>
                    </div>

                    <div class="chat__watchman" v-if="showWatchman">
                        <span>Вахтёр присоединился к чату</span>
                    </div>

                    <!-- <div class="chat__note left-note" v-show="showNote2" @animationend="handleNoteAnimationEnd">
                <div class="chat__note-avatar">
                    <img src="@/assets/ava_ded.png" alt="avatar" class="avatars">
                </div>
                <div class="chat__note-pen">
                    <img src="@/assets/pen.svg" alt="avatar" class="pen-animation">
                    <img src="@/assets/paper.svg" alt="avatar">
                </div>
            </div> -->

                    <div class="chat__left" v-show="showLeftSecond">
                        <div ref="avatar" class="chat__left-avatar show-left-1">
                            <img src="@/assets/ava_ded.png" alt="avatar" class="avatars">
                        </div>
                        <div class="chat__left-bubble left-text-2">
                            <TheChatBlue :isLast="true">
                                <template #chattng__text>
                                    <div class="chat__left-bubble_inner">
                                        <span>У меня пара вопросов. <br> Ответишь — пропущу.</span>
                                    </div>
                                </template>
                            </TheChatBlue>
                        </div>
                    </div>

                    <div ref="scrollAnchor">
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
    methods: {
        handleNoteAnimationEnd() {
            this.showNote = false;
        },
        goTo() {
            window.location.href = '/games/quiz';
        },
        scrollToElement() {
            this.$nextTick(() => {
                const chat = document.querySelector('.chat');
                if (chat) {
                    chat.scrollTo({
                        top: chat.scrollHeight,
                        behavior: 'smooth'
                    });
                }
            });
        },
    },

    mounted() {
        // document.body.style.overflow = 'auto';
        let el = this.$refs.avatar;
        setTimeout(() => {
            this.showNote = true;
        }, 1000);

        setTimeout(() => {
            this.showLeft = true;
        }, 2000);

        setTimeout(() => {
            this.showRight = true;
        }, 3000);

        setTimeout(() => {
            this.showWatchman = true;
        }, 5000);

        setTimeout(() => {
            this.showNote2 = true;
        }, 6000);

        setTimeout(() => {
            this.showNote2 = false;
            this.showLeftSecond = true;
            // if (el) {
            //         el.style.transition = 'transform 0.5s ease, opacity 0.5s ease';
            //         el.style.opacity = '1'; // Убедитесь, что аватар виден
            //         el.style.transform = `translateY(${finalIndex * 100}px)`; // Перемещение вниз
            //     }
        }, 5000);

        setTimeout(() => {
            this.showButton = true;
            this.scrollToElement();
        }, 7000);


    },
    data() {
        return {
            showLeft: false,
            showNote: false,
            showNote2: false,
            showRight: false,
            showWatchman: false,
            showLeftSecond: false,
            showButton: false,
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
    margin: 0 0 10px 0;
    height: 60vh;
    overflow: auto;
    // margin: 0 0 20px 0;

    &::-webkit-scrollbar {
        height: 0;
        width: 0;
    }

    @media (max-width: 1920px) {
        padding: 20px 7px 0;
    }

    @media (max-width: 800px) {
        padding: 20px 7px 50px;
    }

    &__left {
        display: flex;
        gap: 6px;
        justify-content: start;
        align-items: start;
        margin: 13px 0 0 0;

        &-avatar {
            padding: 30px 0 0 0;
            // opacity: 0;
        }

        &-bubble {
            display: flex;
            flex-direction: column;
            gap: 4px;
            align-items: center;
            margin: 0 18px 0 0;

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

    &__right {
        display: flex;
        gap: 6px;
        justify-content: end;
        align-items: end;

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
            margin: 0 0 20px 0;
            // opacity: 0;
        }

        &-avatar {
            display: flex;
            justify-content: flex-end;
            // opacity: 0;
        }
    }

    &__right_2 {
        display: flex;
        gap: 6px;
        justify-content: end;
        align-items: end;
        margin: 26px 0 0 0;

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
            margin: 0 0 20px 0;
            // opacity: 0;
        }

        &-avatar {
            display: flex;
            justify-content: flex-end;
            // opacity: 0;
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
        margin: 65px 0 0 0;
        white-space: nowrap;
        cursor: pointer;

        position: absolute;
        bottom: 50px;
        left: 7px;
        width: calc(100% - 14px);

        @media (max-width: 430px) {
            margin: 65px 0 0 0;
        }

        @media (max-width: 425px) {
            margin: 65px 0 0 0;
        }

        @media (max-width: 420px) {
            margin: 65px 0 0 0;
        }
    }

    &__watchman {
        border-radius: 24px;
        padding: 4px 10px;
        // height: 23px;
        backdrop-filter: blur(13.483146667480469px);
        background: rgba(255, 255, 255, 0.3);
        margin: 27px 80px;
        white-space: nowrap;
        display: flex;
        justify-content: center;
        align-items: center;

        span {
            font-family: var(--gte);
            font-weight: 500;
            font-size: 13px;
            line-height: 120%;
            color: #fff;
            text-align: center;
        }
    }

    .avatars {
        width: 40px;
    }
}

@keyframes penMove {
    0% {
        transform: translateX(0);
    }

    50% {
        transform: translateX(10px);
    }

    100% {
        transform: translateX(0);
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

.left-note {
    animation: fadeOut 0.3s ease-in forwards;
    animation-delay: 1s;
    opacity: 1;
}

.pen-animation {
    animation: penMove 0.5s ease-in-out infinite;
}

.right-avatar-1 {
    animation: fadeIn 0.3s ease-in forwards;
    animation-delay: 1s;
    opacity: 0;
}

.right-text-1 {
    animation: fadeIn 0.3s ease-in forwards;
    animation-delay: 1s;
    opacity: 0;
}

.show-left-1 {
    animation: fadeIn 0.3s ease-in forwards;
    animation-delay: 1s;
    opacity: 0;
}

.show-right-1 {
    animation: fadeIn 0.3s ease-in forwards;
    animation-delay: 1s;
    opacity: 0;
}

.left-text-2 {
    justify-content: flex-end;
    padding-right: 120px;
    animation: fadeIn 0.3s ease-in forwards;
    animation-delay: 1s;
    opacity: 0;
}
</style>
