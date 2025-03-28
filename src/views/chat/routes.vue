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
                    <div class="chat__left">
                        <div class="chat__left-avatar avatar-1">
                            <img src="@/assets/ava_g.png" alt="avatar" class="avatars">
                        </div>
                        <div class="chat__left-bubble">
                            <TheChatBlue :isLast="true">
                                <template #chattng__text>
                                    <div class="chat__left-bubble_inner">
                                        <img src="@/assets/r.png" alt="avatar">
                                        <span>Коробчик, ты готов? Водитель сейчас подъедет</span>
                                    </div>
                                </template>
                            </TheChatBlue>
                        </div>
                    </div>

                    <div class="chat__right show-right-1" v-show="showRight">
                        <div class="chat__right-bubble right-text-1">
                            <TheChatWhite>
                                <template #chattng__text>
                                    <span>Торопишь, чтобы не платить за ожидание?</span>
                                </template>
                            </TheChatWhite>
                        </div>
                        <div class="chat__right-avatar right-avatar-1">
                            <img src="@/assets/ava_b.png" alt="avatar" class="avatars">
                        </div>
                    </div>

                    <div class="chat__note left-note" v-show="showNote" @animationend="handleNoteAnimationEnd">
                        <!-- <div class="chat__note-avatar">
                    <img src="@/assets/ava_g.png" alt="avatar" class="avatars">
                </div> -->
                        <!-- <div class="chat__note-pen">
                    <img src="@/assets/pen.svg" alt="avatar" class="pen-animation">
                    <img src="@/assets/paper.svg" alt="avatar">
                </div> -->
                    </div>

                    <div class="chat__left show-left-1" v-show="showLeftSecond">
                        <div class="chat__left-avatar" ref="avatar">
                            <img src="@/assets/ava_g.png" alt="avatar" class="avatars">
                        </div>
                        <div class="chat__left-bubble">
                            <TheChatBlue :isLast="true && !finalMessageAdded">
                                <template #chattng__text>
                                    <div class="chat__left-bubble_inner">
                                        <span>Нет, Коробчик, чтобы его превзойти! </span>
                                    </div>
                                </template>
                            </TheChatBlue>
                            <TheChatBlue :isLast="true" v-if="clickedTap">
                                <template #chattng__text>
                                    <div class="chat__left-bubble_inner">
                                        <span>Ещё на этапе заказа с сайта мы знаем, когда доставим товар и какой курьер
                                            это
                                            сделает.</span>
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

                    <div class="chat__right show-right-2" v-if="showRightSecond">
                        <div class="chat__right-bubble">
                            <TheChatWhite>
                                <template #chattng__text>
                                    <span>Ого, как это?</span>
                                </template>
                            </TheChatWhite>
                        </div>
                        <div class="chat__right-avatar right-avatar-2">
                            <img src="@/assets/ava_b.png" alt="avatar" class="avatars">
                        </div>
                    </div>

                    <div class="chat__note left-note" v-show="showNote2" @animationend="handleNoteAnimationEnd">
                        <!-- <div class="chat__note-avatar">
                    <img src="@/assets/ava_g.png" alt="avatar" class="avatars">
                </div> -->
                        <!-- <div class="chat__note-pen">
                    <img src="@/assets/pen.svg" alt="avatar" class="pen-animation">
                    <img src="@/assets/paper.svg" alt="avatar">
                </div> -->
                    </div>

                    <div class="chat__left" v-show="showLeftThird">
                        <div ref="last_avatar">
                            <img src="@/assets/ava_g.png" alt="avatar" class="avatars">
                        </div>
                        <div class="chat__left-bubble">
                            <TheChatBlue v-for="(message, index) in chatMessages" :key="index"
                                v-show="showChatBlue[index]" class="chat_width">
                                <template #chattng__text>
                                    <div class="chat__left-bubble_inner">
                                        <span>{{ message }}</span>
                                    </div>
                                </template>
                            </TheChatBlue>
                            <TheChatBlue :isLast="true" class="chat_width" v-show="lastMessage">
                                <template #chattng__text>
                                    <div class="chat__left-bubble_inner">
                                        <img src="@/assets/z.png" alt="pic">
                                        <span>Это твоё следующее задание!</span>
                                    </div>
                                </template>
                            </TheChatBlue>
                        </div>
                    </div>

                    <div ref="scrollAnchor"></div>

                    <div ref="scrollAnchor1" v-show="lastMessage">
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
    mounted() {
        setTimeout(() => {
            this.showRight = true;
        }, 1000);

        // setTimeout(() => {
        //     this.showNote = true;
        // }, 4000);

        setTimeout(() => {
            this.showLeftSecond = true;
        }, 2000);

        setTimeout(() => {
            this.showTap = true;
        }, 4000);
    },
    data() {
        return {
            showRight: false,
            showNote: false,
            showNote2: false,
            showLeftSecond: false,
            showLeftThird: false,
            chatMessages: [
                "Всё благодаря системе «Маршрутыч».",
                "Сначала территории доставки разбивают на полигоны с учётом количества заказов в день и нагрузки курьера, а после прокладывают маршруты до нужных адресов."
            ],
            showChatBlue: [false, false, false],
            showButton: false,
            showTap: false,
            clickedTap: false,
            finalMessageAdded: false,
            showRightSecond: false,
            lastMessage: false,
        }
    },
    methods: {
        scrollToElement(refName) {
            this.$nextTick(() => {
                const targetElement = this.$refs[refName];
                if (targetElement) {
                    targetElement.scrollIntoView({ behavior: 'smooth', block: 'end' });
                }
            });
        },
        handleNoteAnimationEnd() {
            this.showNote = false;
        },
        addNewChat() {
            let el = this.$refs.avatar;
            // el.classList.add('avatar-padding');
            if (el) {
                el.style.transition = 'transform 0.5s ease, opacity 0.5s ease';
                el.style.opacity = '1'; // Убедитесь, что аватар виден
                el.style.transform = `translateY(${1 * 80}px)`; // Перемещение вниз
            }
            this.clickedTap = true;
            this.finalMessageAdded = true;
            this.showTap = false;
            setTimeout(() => {
                this.showRightSecond = true;
            }, 0);
            setTimeout(() => {
                this.showNote2 = true;
            }, 0);
            setTimeout(() => {
                this.showNote2 = false;
                this.showLeftThird = true;
                this.showChatMessages();

                // this.scrollToElement("scrollAnchor1");
            }, 2000);
        },
        showChatMessages() {
            let el = this.$refs.last_avatar;
            // setTimeout(() => {
            //     el.classList.add('avatar-padding-1');
            // }, 0);
            // setTimeout(() => {
            //     el.classList.add('avatar-padding-2');
            // }, 1000);
            // setTimeout(() => {
            //     el.classList.add('avatar-padding-3');
            // }, 2000);  //1 3 4

            setTimeout(() => {
                this.chatMessages.forEach((message, index) => {
                    setTimeout(() => {
                        this.showChatBlue[index] = true;
                        if (el) {
                            el.style.transition = 'transform 0.5s ease, opacity 0.5s ease';
                            el.style.opacity = '1'; // Убедитесь, что аватар виден
                            el.style.transform = `translateY(${index * 110}px)`; // Перемещение вниз
                        }
                        this.scrollToElement("scrollAnchor");
                    }, index * 1000);

                    setTimeout(() => {
                        this.lastMessage = true;
                        this.scrollToElement("scrollAnchor1");
                        el.style.paddingBottom = '50px';
                        el.style.transition = 'transform 0.5s ease, opacity 0.5s ease';
                        el.style.opacity = '1'; // Убедитесь, что аватар виден
                        el.style.transform = `translateY(${3 * 120}px)`; // Перемещение вниз
                        this.showButton = true;


                    }, 2000);
                });
            }, 0);
        },
        goTo() {
            window.location.href = '/games/routs';
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
    height: 100vh;
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
    padding: 90px 7px 0;
    height: 85dvh;
    overflow: auto;

    &::-webkit-scrollbar {
        height: 0;
        width: 0;
    }

    @media (max-width: 1920px) {
        padding: 20px 7px 0;
    }

    @media (max-width: 800px) {
        padding: 20px 7px 0;
    }

    &__left {
        display: flex;
        gap: 5px;
        justify-content: start;
        align-items: start;
        margin: 13px 0 0 0;

        &-avatar {
            padding: 10px 0 0 0;
        }

        &-bubble {
            display: flex;
            flex-direction: column;
            gap: 4px;
            align-items: start;

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
        width: 100%;
        margin: 50px 0 50px 0;
        white-space: nowrap;
        cursor: pointer;
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

.left-text-1 {
    position: absolute;
    left: 70px;
    top: 100px;
}

.left-note {
    animation: fadeOut 0.3s ease-in forwards;
    animation-delay: 1s;
    opacity: 1;
}

.pen-animation {
    animation: penMove 0.5s ease-in-out infinite;
}

.chat_width {
    width: 256px;
}

.tap {
    position: fixed;
    bottom: 47px;
    right: 23px;
    animation: heartbeat 1.5s infinite;
    cursor: pointer;

    @media (max-width: 1920px) {
        right: 730px;
    }

    @media (max-width: 1440px) {
        right: 550px;
    }

    @media (max-width: 800px) {
        bottom: 10px;
        right: 23px;
    }

    @media (max-width: 430px) {
        bottom: 47px;
    }
}

.show-right-1 {
    animation: fadeIn 0.3s ease-in forwards;
    animation-delay: 1s;
    opacity: 0;
}

.show-right-2 {
    animation: fadeIn 0.3s ease-in forwards;
    animation-delay: 1s;
    opacity: 0;
}

.show-left-1 {
    animation: fadeIn 0.3s ease-in forwards;
    animation-delay: 1s;
    opacity: 0;
}

.avatar-1 {
    padding: 230px 0 0 0;
}

.avatar-padding {
    padding: 80px 0 0 0;
}

.avatar-padding-1 {
    padding: 10px 0 0 0;
}

.avatar-padding-2 {
    padding: 110px 0 0 0;
}

.avatar-padding-3 {
    padding: 350px 0 0 0;
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
</style>
