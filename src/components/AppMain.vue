<template>
    <main>

        <div class="container">
            <select @change="queryServer($event)" class="my-4" ref="select">
                <option value="?num=50&offset">50 Cards</option>
                <option value="?archetype=Alien">Alien</option>
                <option value="?archetype=Exodia">Exodia</option>
                <option value="?archetype=Melodious">Melodius</option>
                <option value="?archetype=Archfiend">Archfiend</option>
                <option value="?archetype=Blue-Eyes">Blue Eyes</option>
                <option value="?archetype=Umi">Umi</option>
                <option value="?archetype=ABC">ABC</option>
                <option value="?archetype=A.I.">A.I.</option>
                <option value="?archetype=@Ignister">@Ignister</option>
                <option value="?archetype=Unchained">Unchained</option>
            </select>
        </div>

        <div class="container al-white">
            <div class="row al-p justify-content-center">
                <h3 class="al-black">Found {{ dim }} {{ witchWord() }}</h3>
                <div>
                    <div class="row">
                        <div v-for="cardData in cardsData" class="col-12 col-sm-6 col-md-4 col-lg-3 col-xxl-2 pb-5">
                        <AppCard :cardData="cardData" class="h-100" />
                    </div>
                </div>
                </div>
            </div>
        </div>
    </main>
</template>

<script>
    import AppCard from './AppCard.vue';

    export default {
        name: "AppMain",

        data() {
            return {
                cardsData: [],
                dim: 0
            }
        },

        components: {
            AppCard
        },

        methods: {
            getCards(){
                axios.get("https://db.ygoprodeck.com/api/v7/cardinfo.php?num=50&offset").then( (response) => {
                    this.dim = response.data.data.length;
                    for(let i = 0; i < response.data.data.length; i++)
                        this.cardsData.push({imgSrc: response.data.data[i].card_images[0].image_url, cardName: response.data.data[i].name, cardArchetype: response.data.data[i].archetype});
                });
            },

            witchWord(){
                let result = "card";

                if(this.dim > 1)
                    result += "s";

                return result;
            },

            queryServer(e){
                let query = "https://db.ygoprodeck.com/api/v7/cardinfo.php" + e.target.value;
                this.cardsData = [];
                axios.get(query).then( (response) => {
                    this.dim = response.data.data.length;
                    for(let i = 0; i < response.data.data.length; i++)
                        this.cardsData.push({imgSrc: response.data.data[i].card_images[0].image_url, cardName: response.data.data[i].name, cardArchetype: response.data.data[i].archetype});
                });
            }
        },

        mounted() {
            this.getCards();
        }
    }
</script>

<style lang="scss" scoped>
    @import "../assets/colors.scss";

    main{
        background-color: $main_color;
    }

    .al-white{
        background-color: white;
    }

    .al-black{
        background-color: #212529;
        color: white;
        margin-bottom: 0;
        padding: 0.6rem 0.5rem;
        width: calc(100% - 1.5rem);
    }

    .al-p{
        padding: 2rem;
    }
</style>