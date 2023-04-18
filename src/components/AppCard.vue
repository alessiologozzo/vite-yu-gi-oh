<template>
    <div class="al-card">
        <div>
            <img :src="cardData.imgSrc" alt="err">
        </div>
        <div class="text-center">
            <h4 class="al-white pt-2">{{ cardData.cardName }}</h4>
            <h6 class="pt-2 pb-3">{{ cardData.cardArchetype}}</h6>
        </div>
    </div>
</template>

<script>
    export default {
        name: "AppCard",

        props: {
            cardData: {imgSrc: String, cardName: String, cardArchetype: String}
        },
    }
</script>

<style lang="scss" scoped>
    @import "../assets/colors.scss";

    .al-card{
        background-color: $main_color;
        width: 100%;
    }

    img{
        object-fit: cover;
        width: 100%;
        height: 100%;
    }

    .al-white{
        color: white;
    }
</style>