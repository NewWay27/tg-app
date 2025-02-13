<template>
    <div class="loader">
        <img src="@/assets/logoo.svg" alt="logo" class="logo">
        <img src="@/assets/load.svg" alt="logo" class="spinner">
    </div>

</template>


<style lang="scss">
.loader {
    background: #0a3956;
    height: 100vh;
    width: 539px;
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 100px;

    @media (max-width: 1920px) {
        height: 90vh;
        border-radius: 29px 29px 0 0;
    }

    @media (max-width: 1440px) {
        width: 390px;
    }

    @media (max-width: 480px) {
        height: 100vh;
        width: 100%;
        border-radius: 0;
    }

    .logo {
        margin: 150px 0 0 0;
    }

    .spinner {
        animation: spin 1s linear infinite;
    }
}


@keyframes spin {
    to {
        transform: rotate(360deg);
    }
}
</style>