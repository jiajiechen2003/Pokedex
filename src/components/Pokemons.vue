<template>
    <div class="container-fluid">
        <div class="card-col" v-for="pokemon in pokemons" :key="pokemon.name">
            <div class="card pokemon-card">
                <div class="card-body">
                    <img :src="pokemon.sprites.other['official-artwork'].front_default">
                    <div class="pokemon-info">
                        <div class="pokemon-data">
                            <p>{{ formattedId(pokemon.id) }}</p>
                            <label :class="{ 'favorite': pokemon.favorite, 'add-favorite': !pokemon.favorite }"
                                @click="markPokemonAsFavorite(pokemon)">
                                <input type="radio" :checked="pokemon.favorite">
                            </label>
                        </div>
                        <div class="name">
                            <h5 class="pokemon-name">{{ pokemon.name }}</h5>
                            <button @click="addPokemonToTeam(pokemon)">{{ pokemon.team ? 'Remove Team' : 'Add Team'
                                }}</button>
                        </div>
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
            pokemons: [],
            favorites: [],
            team: []
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
                                if (this.favorites.includes(pokeData.id)) {
                                    pokeData.favorite = true;
                                }
                                if (this.team.includes(pokeData.id)) {
                                    pokeData.team = true;
                                }
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
        markPokemonAsFavorite(pokemon) {
            pokemon.favorite = !pokemon.favorite;
            if (pokemon.favorite) {
                this.favorites.push(pokemon.id);
            } else {
                const index = this.favorites.indexOf(pokemon.id);
                if (index !== -1) {
                    this.favorites.splice(index, 1);
                }
            }
            localStorage.setItem('favorites', JSON.stringify(this.favorites));
        },
        loadFavorites() {
            this.favorites = JSON.parse(localStorage.getItem('favorites')) || [];
        },
        addPokemonToTeam(pokemon) {
            if (pokemon.team) {
                const index = this.team.indexOf(pokemon.id);
                if (index !== -1) {
                    this.team.splice(index, 1);
                    pokemon.team = false; // Actualiza el estado del equipo del Pokémon
                    localStorage.setItem('team', JSON.stringify(this.team));
                }
            } else {
                if (this.team.length < 6) {
                    pokemon.team = true; // Actualiza el estado del equipo del Pokémon
                    this.team.push(pokemon.id);
                    localStorage.setItem('team', JSON.stringify(this.team));
                } else {
                    alert("¡El equipo ya tiene 6 Pokémon!");
                }
            }
        },
        loadTeam() {
            this.team = JSON.parse(localStorage.getItem('team')) || [];
        }
    },
    created() {
        this.selectPokemons();
        this.loadFavorites();
        this.loadTeam();
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

.favorite {
    width: 30px;
    height: 30px;
    background-image: url('../img/heart-fill.svg');
    background-size: cover;
    cursor: pointer;
    border: none;
}

.add-favorite {
    width: 30px;
    height: 30px;
    background-image: url('../img/heart.svg');
    background-size: cover;
    cursor: pointer;
    border: none;
}

.add-favorite input,
.favorite input {
    width: 30px;
    height: 30px;
    opacity: 0;
    cursor: pointer;
}

.name {
    display: flex;
    flex-direction: row;
    align-items: center;
    gap: 5px;
}

.name button {
    height: 18px;
    width: 80px;
    display: flex;
    justify-content: center;
    align-items: center;
    font-size: 11px;
}
</style>