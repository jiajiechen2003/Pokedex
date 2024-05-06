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
        formattedId(id) {
            return "Nº" + id.toString().padStart(4, "0");
        },
        pokemonTypeColor(type) {
            switch (type) {
                case 'grass':
                    return { backgroundColor: '#98CC50' };
                case 'poison':
                    return { backgroundColor: '#B97FC9' };
                case 'fire':
                    return { backgroundColor: '#FD7D24' };
                case 'flying':
                    return { background: 'linear-gradient(180deg, #3dc7ef 50%, #bdb9b8 50%)' };
                case 'water':
                    return { backgroundColor: '#4592c4' };
                case 'bug':
                    return { backgroundColor: '#729f3f' };
                case 'normal':
                    return { backgroundColor: '#A4ACAF' };
                case 'electric':
                    return { backgroundColor: '#EED535' };
                case 'ground':
                    return { background: 'linear-gradient(180deg, #f7de3f 50%, #ab9842 50%)' };
                case 'fairy':
                    return { backgroundColor: '#fd89e9' };
                case 'ice':
                    return { backgroundColor: '#51C4E7' };
                case 'fighting':
                    return { backgroundColor: '#D56723' };
                case 'psychic':
                    return { backgroundColor: '#F366B9' };
                case 'rock':
                    return { backgroundColor: '#A38C21' };
                case 'ghost':
                    return { backgroundColor: '#7B62A3' };
                case 'steel':
                    return { backgroundColor: '#9EB7B8' };
                case 'dragon':
                    return { backgroundColor: '#7B62A3' };
            }
        }
    },
    created() {
        this.selectPokemons()
    }
}
</script>

<template>
    <div class="container-fluid">
        <div class="card-col" v-for="pokemon in pokemons" :key="pokemon.name">
            <div class="card pokemon-card">
                <div class="card-body">
                    <img :src="pokemon.sprites.other['official-artwork'].front_default">
                    <div class="pokemon-info">
                        <p>{{ formattedId(pokemon.id) }}</p>
                        <h5 class="pokemon-name">{{ pokemon.name }}</h5>
                        <div class="types">
                            <div class="type" v-for="type in pokemon.types" :key="type.type.name"
                                :style="pokemonTypeColor(type.type.name)">
                                <h5>{{ type.type.name }}</h5>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>



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

.type {
    border-radius: 3px;
    background-color: red;
    width: 100px;
    justify-content: center;
    display: flex;
    align-items: center;
}

.type h5 {
    font-size: 0.9rem
}
</style>