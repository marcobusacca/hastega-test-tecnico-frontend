<script>
export default {

}
</script>

<template>
    <div class="center">
        <div class="ring"></div>
        <span class="loader-text">loading...</span>
    </div>
</template>

<style lang="scss">
@use '../styles/generals.scss' as *;

.center {
    display: flex;
    text-align: center;
    justify-content: center;
    align-items: center;
    padding: 100px 0;
}

.ring {
    position: absolute;
    width: 200px;
    height: 200px;
    border-radius: 50%;
    animation: ring 2s linear infinite;

}

@keyframes ring {
    0% {
        transform: rotate(0deg);
        box-shadow: 1px 5px 2px #e65c00;
    }

    50% {
        transform: rotate(180deg);
        box-shadow: 1px 5px 2px #18b201;
    }

    100% {
        transform: rotate(360deg);
        box-shadow: 1px 5px 2px #0456c8;
    }
}

.loader-text {
    color: black;
    text-transform: uppercase;
    font-size: 20px;
    font-weight: 500;
    line-height: 200px;
    letter-spacing: 1px;
}

@media screen and (min-width: 768px) {
    .center {
        padding: 200px 0;
    }
}
</style>