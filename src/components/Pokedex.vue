<script>
// import axios from 'axios';

export default {
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
    },
    created() {
        this.selectPokemons()
    }
}
</script>

<template>
    <!-- <div>
        <div v-if="pokemons.length > 0">
            <h2>Lista de nombres de Pokémon:</h2>
            <ul>
                <li v-for="pokemon in pokemons" :key="pokemon.name">
                    <strong>{{ pokemon.name }}</strong> - Habilidades:
                    <ul>
                        <li v-for="type in pokemon.types" :key="type.type.name">{{ type.type.name }}</li>
                    </ul>
                </li>
            </ul>
        </div>
    </div> -->
    <div class="container-fluid">
        <div class="row">
            <div class="col-4 card-col" v-for="pokemon in pokemons" :key="pokemon.name">
                <div class="card pokemon-card">
                    <div class="card-header">{{ pokemon.name }}</div>
                    <div class="card-body">
                        <p class="card-text">Some quick example text to build on the card title and make up the bulk of
                            the card's content.</p>
                        <a href="#" class="btn btn-primary">Go somewhere</a>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>



<style scoped>
.container-fluid {
    height: auto;
    background-color: red;
}


.card-col{
    display: flex;
    justify-content: center;
    align-items: center;
    margin-top: 20px;
}

.pokemon-card {
    width: 320px;
    height: 400px;
}
</style>