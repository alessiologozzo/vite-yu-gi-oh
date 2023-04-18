<template>
    <main>

        <div class="container">
            <select @change="onChange($event)" class="my-4" ref="select">
                <option value="all">All</option>
                <option value="Alien">Alien</option>
                <option value="Noble Knight">Noble Knight</option>
                <option value="Melodious">Melodius</option>
                <option value="Archfiend">Archfiend</option>
                <option value="Elemental HERO">Elemental HERO</option>
                <option value="Umi">Umi</option>
                <option value="ABC">ABC</option>
                <option value="A.I.">A.I.</option>
                <option value="@Ignister">@Ignister</option>
                <option value="Unchained">Unchained</option>
                <option value="noArch">No Archetype</option>
            </select>
        </div>

        <div class="container al-white">
            <div class="row al-p justify-content-center">
                <h3 class="al-black">Found {{ dim }} {{ witchWord() }}</h3>
                <div>
                    <div class="row">
                        <div v-for="cardData in cardsData" class="col-12 col-sm-6 col-md-4 col-lg-3 col-xxl-2 pb-5" :class="{'d-none': !hasToShow(cardData)}">
                        <AppCard :cardData="cardData" class="h-100" v-if="hasToShow(cardData)"/>
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
            
            onChange(e){
                    this.dim = 0;

                    for(let i = 0; i < this.cardsData.length; i++)
                        if(e.target.value == this.cardsData[i].cardArchetype)
                            this.dim++;
                    
                },

            hasToShow(cardData){
                let result = false;
                let select = this.$refs.select;
                
                if(select.value == "all")
                    result = true;
                else if(select.value == "noArch" && cardData.cardArchetype == undefined)
                    result = true;
                else if(select.value == cardData.cardArchetype)
                    result = true;

                return result;
            },

            witchWord(){
                let result = "card";

                if(this.dim > 1)
                    result += "s";

                return result;
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