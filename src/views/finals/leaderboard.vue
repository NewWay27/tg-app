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
                    <div class="header__logo-back">
                        <img src="@/assets/znak.svg" alt="symbol" @click="goBack">

                        <div class="header__logo">
                            <img src="@/assets/brace.svg" alt="logo">
                            <div class="header__logo-text">
                                <div>
                                    <span>Ozon Tech</span>
                                    <img src="@/assets/galka.svg" alt="item">
                                </div>
                                <span>Таблица лидеров</span>
                            </div>
                        </div>
                    </div>
                    <div class="header__menu" @click="openMenu">
                        <img src="@/assets/hamb.svg" alt="menu">
                        <span>Меню</span>
                    </div>
                </div>

                <div class="leaders-top">
                    <div class="leader">
                        <img src="@/assets/2.svg" alt="2">
                        <div>
                            <p>{{ second_leader }}</p>
                            <span>{{ second_leader_points }}</span>
                        </div>
                    </div>
                    <div class="leader first-place">
                        <img src="@/assets/1.svg" alt="1">
                        <div>
                            <p>{{ first_leader }}</p>
                            <span>{{ first_leader_points }}</span>
                        </div>
                    </div>
                    <div class="leader">
                        <img src="@/assets/3.svg" alt="3">
                        <div>
                            <p>{{ third_leader }}</p>
                            <span>{{ third_leader_points }}</span>
                        </div>
                    </div>
                </div>

                <div class="leaders-list">
                    <div class="leaders-list__item" v-for="(leader, index) in leaders.slice(3)" :key="index + 3">
                        <div class="leaders-list__item-inner">
                            <div>
                                <p>{{ index + 4 }}</p>
                                <img src="@/assets/4.svg" alt="avatar">
                            </div>
                            <span>{{ leader.username }}</span>
                        </div>
                        <span>{{ leader.totalPoints }}</span>
                    </div>

                    <!-- <div class="leaders-list__item">
                <div class="leaders-list__item-inner">
                    <div>
                        <p>19</p>
                        <img src="@/assets/4.svg" alt="avatar">
                    </div>
                    <span>ываыва</span>
                </div>
                <span>999</span>
            </div> -->
                    <!-- <div class="leaders-list__item">
                <div class="leaders-list__item-inner">
                    <div>
                        <p>20</p>
                        <img src="@/assets/4.svg" alt="avatar">
                    </div>
                    <span>ываыва</span>
                </div>
                <span>999</span>
            </div> -->

                    <div v-if="isHiddenElementVisible" class="user-fixed" id="user-place1">
                        <div class="leaders-list__item-inner">
                            <div>
                                <p>{{ userPlaceNumber }}</p>
                                <img src="@/assets/last.svg" alt="avatar">
                            </div>
                            <span>{{ currentLeader.username }}</span>

                        </div>
                        <span>{{ currentLeader.points }}</span>
                    </div>
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
                        <div @click="openVacancies">
                            <img src="@/assets/op.svg" alt="item">
                            <span>Вакансии Ozon Tech</span>
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
import axios from 'axios';

export default {
    components: {
        TheMenu
    },
    data() {
        return {
            showMenu: false,
            userPlaceNumber: null,
            isHiddenElementVisible: false,
            leaders: [],
            currentLeader: [],
            first_leader: '',
            second_leader: '',
            third_leader: '',
            first_leader_points: '',
            second_leader_points: '',
            third_leader_points: '',
        };
    },
    mounted() {
        this.getLeaders();
        this.getMyPoints();
        this.checkVisibility();
        window.addEventListener('scroll', this.checkVisibility);
    },
    beforeDestroy() {
        window.removeEventListener('scroll', this.checkVisibility);
    },
    methods: {
        openChat() {
            this.$router.push('/chat/obx');
            document.body.style.overflow = '';
        },
        openTelegram() {
            window.open('https://t.me/s/ozon_tech', '_blank', 'noopener,noreferrer');
        },
        openVacancies() {
            window.location.href = 'https://ozon.tech/gamebot-job';
        },
        getCookie(name) {
            const value = `; ${document.cookie}`;
            const parts = value.split(`; ${name}=`);
            if (parts.length === 2) return parts.pop().split(';').shift();
        },
        getLeaders() {
            const params = this.getCookie("uuid");

            if (!params) {
                console.error("Куки с именем 'uuid' не найдены.");
                return;
            }
            console.log(params, ' UUID')

            let url = 'https://ozontechhrbot.ru/api/leaderboard';

            axios
                .get(url, {
                    params: {
                        "code": params,
                    },
                    headers: {
                        "Content-Type": "application/json",
                        "Accept": "application/json",
                    },
                })
                .then(res => {
                    console.log(res.data);
                    console.log('alo')
                    this.leaders = res.data.data;
                    this.first_leader = res.data.data[0].username;
                    this.second_leader = res.data.data[1].username;
                    this.third_leader = res.data.data[2].username;
                    this.first_leader_points = res.data.data[0].totalPoints;
                    this.second_leader_points = res.data.data[1].totalPoints;
                    this.third_leader_points = res.data.data[2].totalPoints;

                })
                .catch(error => {
                    console.error(error);
                })
        },
        getMyPoints() {
            const params = this.getCookie("uuid");

            if (!params) {
                console.error("Куки с именем 'uuid' не найдены.");
                return;
            }

            let url = 'https://ozontechhrbot.ru/api/leaderboard/me';

            axios
                .get(url, {
                    params: {
                        "code": params,
                    },
                    headers: {
                        "Content-Type": "application/json",
                        "Accept": "application/json",
                    },
                })
                .then(res => {
                    console.log(res.data);
                    this.currentLeader = res.data;
                    this.userPlaceNumber = res.data.rank;
                })
                .catch(error => {
                    console.error(error);
                })
        },
        openMenu() {
            this.showMenu = true;
            document.body.style.overflow = 'hidden';
        },
        goBack() {
            this.$router.push('/finals/final');
        },
        checkVisibility() {
            const leaderItems = Array.from(document.querySelectorAll('.leaders-list__item'));
            let userPlace = null;
            let previousPlace = null;

            // Находим элементы пользователя и предыдущего места
            leaderItems.forEach(item => {
                const placeNumber = parseInt(item.querySelector('p').textContent.trim(), 10);
                if (placeNumber === this.userPlaceNumber) {
                    userPlace = item;
                } else if (placeNumber === this.userPlaceNumber - 1) {
                    previousPlace = item;
                }
            });

            if (!userPlace) return;
            const rect = userPlace.getBoundingClientRect();
            const isOutOfView = rect.bottom < 0 || rect.top > window.innerHeight;

            // Обновляем состояние видимости
            this.isHiddenElementVisible = isOutOfView;
        }



        // checkPosition() {
        //     const leaderItems = Array.from(document.querySelectorAll('.leaders-list__item'));
        //     let userPlace = null;
        //     let previousPlace = null;

        //     leaderItems.forEach(item => {
        //         const placeNumber = parseInt(item.querySelector('p').textContent.trim(), 10);
        //         if (placeNumber === this.userPlaceNumber) {
        //             userPlace = item;
        //         } else if (placeNumber === this.userPlaceNumber - 1) {
        //             previousPlace = item;
        //         }
        //     });

        //     if (userPlace && previousPlace && this.userPlaceNumber >= 10) {
        //         const rect = userPlace.getBoundingClientRect();
        //         if (rect.top >= 0 && rect.bottom <= window.innerHeight) {
        //             previousPlace.after(userPlace);
        //             userPlace.classList.remove('user-fixed');
        //             userPlace.style.boxShadow = 'none';
        //         } else {
        //             userPlace.classList.add('user-fixed');
        //         }
        //     }
        // }
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
}

.page {
    overflow: hidden;
    position: relative;
    background-color: #02283E;
    background-image: url(@/assets/Vector.png);
    background-size: cover;
    display: flex;
    flex-direction: column;

    height: 100dvh;

    @media (max-width: 1920px) {
        height: 90dvh;
    }

    @media (max-width: 800px) {
        height: 100dvh;
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
    padding: 16px 20px 16px 23px;

    @media (max-width: 1920px) {
        position: static;
    }

    @media (max-width: 800px) {
        position: fixed;
    }

    &__logo-back {
        display: flex;
        gap: 20px;
        align-items: center;
    }


    &__logo {
        display: flex;
        gap: 10px;
        align-items: center;


        &-text {
            span {
                font-family: var(--gte);
                font-weight: 400;
                font-size: 12px;
                line-height: 120%;
                color: #989ba0;
            }

            div {
                display: flex;
                align-items: center;
                gap: 4px;

                span {
                    font-family: var(--gte);
                    font-weight: 500;
                    font-size: 14px;
                    line-height: 120%;
                    color: #fff;
                }
            }
        }
    }

    &__menu {
        border-radius: 25px;
        padding: 0 12px;
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
}

.leaders-top {
    display: flex;
    gap: 131px;
    justify-content: center;
    align-items: center;
    margin: 150px 0 0 0;

    @media (max-width: 1920px) {
        margin: 100px 0 0 0;
    }

    @media (max-width: 800px) {
        margin: 150px 0 0 0;
    }

    .leader {
        display: flex;
        flex-direction: column;
        gap: 8px;

        div {
            display: flex;
            flex-direction: column;
            gap: 4px;

            p {
                font-family: var(--gte);
                font-weight: 500;
                font-size: 13px;
                line-height: 130%;
                text-align: center;
                color: #fff;
                margin: 0;
            }

            span {
                font-family: var(--gte);
                font-weight: 400;
                font-size: 12px;
                line-height: 130%;
                text-align: center;
                color: #fff;
            }
        }
    }

    .first-place {
        position: absolute;
        top: 130px;

        @media (max-width: 1920px) {
            top: 100px;
        }

        @media (max-width: 800px) {
            top: 130px;
        }
    }
}

.leaders-list {
    display: flex;
    flex-direction: column;
    gap: 10px;
    margin: 23px 20px;
    overflow-y: auto;


    &::-webkit-scrollbar {
        height: 0;
        width: 0;
    }

    &__item {
        display: flex;
        justify-content: space-between;
        align-items: center;
        border-radius: 10px;
        padding: 6px 20px 6px 13px;
        background: #383d46;

        &-inner {
            display: flex;
            align-items: center;
            gap: 13px;

            div {
                display: flex;
                align-items: center;
                gap: 7px;

                p {
                    font-family: var(--gte);
                    font-weight: 500;
                    font-size: 13px;
                    line-height: 150%;
                    color: #fff;
                    text-shadow: 0 0 13px 0 rgba(0, 0, 0, 0.25);
                }
            }

            span {
                font-family: var(--gte);
                font-weight: 400;
                font-size: 13px;
                line-height: 150%;
                color: #fff;
            }
        }

        span {
            font-family: var(--gte);
            font-weight: 400;
            font-size: 13px;
            line-height: 150%;
            text-align: right;
            color: #fff;
        }
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

.leaders-list__item {
    transition: transform 0.5s ease, opacity 0.5s ease;
}

.user-place {
    background: #64cc84;
    box-shadow: 0 -7px 12px 10px #02283e;
    transform: translateY(0);
    transition: all 2s ease-in-out;
}

.user-fixed {
    display: flex;
    justify-content: space-between;
    align-items: center;
    border-radius: 10px;
    padding: 6px 20px 6px 13px;
    background: #383d46;

    position: fixed;
    bottom: 20px;
    right: 20px;
    left: 20px;
    transition: all 2s ease-in-out;

    background: #64cc84;
    box-shadow: 0 -7px 12px 10px #02283e;

    span {
        font-family: var(--gte);
        font-weight: 400;
        font-size: 13px;
        line-height: 150%;
        text-align: right;
        color: #fff;
    }
}
</style>
