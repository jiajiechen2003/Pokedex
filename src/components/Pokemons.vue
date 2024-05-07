<template>
    <div class="container-fluid">
        <div class="card-col" v-for="pokemon in pokemons" :key="pokemon.name">
            <div class="card pokemon-card">
                <div class="card-body">
                    <img :src="pokemon.sprites.other['official-artwork'].front_default">
                    <div class="pokemon-info">
                        <div class="pokemon-data">
                            <div class="id">
                                <p>{{ formattedId(pokemon.id) }}</p>
                            </div>
                            <div class="favorite">
                                <input class="fav" type="radio" checked>
                            </div>
                        </div>
                        <h5 class="pokemon-name">{{ pokemon.name }}</h5>
                        <Types class="types" :types="pokemon.types" />
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>
<script>
import Types from './Types.vue';

export default {
    components: {
        Types
    },
    data() {
        return {
            pokemons: []
        };
    },
    methods: {
        selectPokemons() {
            fetch('https://pokeapi.co/api/v2/pokemon?limit=151')
                .then(response => response.json())
                .then(data => {
                    data.results.forEach(pokemon => {
                        fetch(pokemon.url)
                            .then(response => response.json())
                            .then(pokeData => {
                                this.pokemons.push(pokeData);
                            })
                    })
                })
                .catch(error => {
                    console.error('Error fetching data:', error);
                });
        },
        formattedId(id) {
            return "Nº" + id.toString().padStart(4, "0");
        },
    },
    created() {
        this.selectPokemons()
    }
}
</script>
<style scoped>
* {
    margin: 0;
    padding: 0;
    text-transform: capitalize;
}

.container-fluid {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    align-items: center;
    column-gap: 20px;
    row-gap: 50px;
    height: auto;
    background-color: #FFFFFF;
}

.pokemon-info p {
    color: #919191;
    font-family: "Roboto", arial, sans-serif;
    font-size: 100%;
    font-weight: 500;
    line-height: 125%;
    margin: 0.5em 0;
}

.pokemon-name {
    color: #313131;
    font-size: 145%;
    margin-bottom: 5px;
}

.card-col {
    display: flex;
    justify-content: center;
    align-items: center;
    margin-top: 20px;
}

.pokemon-card {
    width: 320px;
    height: 400px;
}

.card-body {
    padding: 0;
    display: flex;
    align-items: center;
    flex-direction: column;
}

.card-body img {
    width: 100%;
    height: 300px;
    background-color: #F2F2F2;
}

.pokemon-data {
    display: flex;
    flex-direction: row;
    justify-content: space-between;
}

.pokemon-info {
    display: flex;
    flex-direction: column;
    width: 100%;
    height: 100%;
    background-color: #FFFFFF;
    padding-left: 7.2525%;
}

.types {
    display: flex;
    flex-direction: row;
    gap: 5px;
}
</style>