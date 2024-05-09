<template>
    <div class="container-fluid">
        <div class="container-fluid">
            <div class="row">
                <div class="col-2 filters">
                    <div class="dropdown">
                        <button class="btn btn-secondary dropdown-toggle" type="button" data-bs-toggle="dropdown"
                            aria-expanded="false">
                            Filter by Type
                        </button>
                        <ul class="dropdown-menu">
                            <li><a class="dropdown-item">Grass</a></li>
                            <li><a class="dropdown-item">Poison</a></li>
                            <li><a class="dropdown-item">Fire</a></li>
                            <li><a class="dropdown-item">Water</a></li>
                            <li><a class="dropdown-item">Bug</a></li>
                            <li><a class="dropdown-item">Flying</a></li>
                            <li><a class="dropdown-item">Normal</a></li>
                            <li><a class="dropdown-item">Electric</a></li>
                            <li><a class="dropdown-item">Ground</a></li>
                            <li><a class="dropdown-item">Fairy</a></li>
                            <li><a class="dropdown-item">Fighting</a></li>
                            <li><a class="dropdown-item">Psychic</a></li>
                            <li><a class="dropdown-item">Rock</a></li>
                            <li><a class="dropdown-item">Steel</a></li>
                            <li><a class="dropdown-item">Ice</a></li>
                            <li><a class="dropdown-item">Ghost</a></li>
                            <li><a class="dropdown-item">Dragon</a></li>
                        </ul>
                    </div>
                    <div class="filters-checks">
                        <div class="team-check">
                            <input type="checkbox" id="onTeam" v-model="showTeam">
                            <label for="onTeam">Team</label>
                        </div>
                        <div class="favorites-check">
                            <input type="checkbox" id="onFavorites" v-model="showFavorites">
                            <label for="onFavorites">Favorites</label>
                        </div>

                        <div class="id-range">
                            <input type="range" class="form-range" id="customRange1">
                        </div>
                    </div>
                </div>
                <div class="col-10 pokemon-display">
                    <nav class="navbar navbar-expand-lg bg-body-tertiary">
                        <div class="container-fluid header">
                            <img src="../img/pokedex-logo.png">
                        </div>
                    </nav>
                    <div class="card-col" v-for="pokemon in filteredPokemons" :key="pokemon.name">
                        <div class="card pokemon-card">
                            <div class="card-body">
                                <img :src="pokemon.sprites.other['official-artwork'].front_default">
                                <div class="pokemon-info">
                                    <div class="pokemon-data">
                                        <p>{{ formattedId(pokemon.id) }}</p>
                                        <label
                                            :class="{ 'favorite': pokemon.favorite, 'add-favorite': !pokemon.favorite }"
                                            @click="markPokemonAsFavorite(pokemon)">
                                            <input type="radio" :checked="pokemon.favorite">
                                        </label>
                                    </div>
                                    <div class="name">
                                        <h5 class="pokemon-name">{{ pokemon.name }}</h5>
                                        <button @click="addPokemonToTeam(pokemon)">{{ pokemon.team ? 'Remove Team' :
                                            'Add Team'
                                            }}</button>
                                    </div>
                                    <Types class="types" :types="pokemon.types" />
                                </div>
                            </div>
                        </div>
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
            team: [],
            showTeam: false,
            showFavorites: false,
            pokemonTypes: []
        };
    },
    computed: {
        filteredPokemons() {
            if (this.showTeam) {
                // if (this.team.length !== 6) {
                    // return this.pokemons;
                    // alert("You need 6 Pokemons on your team")
                // } else {
                    return this.pokemons.filter(pokemon => this.team.includes(pokemon.id));
                // }
            } else if (this.showFavorites) {
                return this.pokemons.filter(pokemon => this.favorites.includes(pokemon.id));
            } else {
                return this.pokemons;
            }
        }
    },
    methods: {
        selectPokemons() {
            fetch('https://pokeapi.co/api/v2/pokemon?limit=151')
                .then(response => response.json())
                .then(data => {
                    Promise.all(data.results.map(pokemon => {
                        return fetch(pokemon.url)
                            .then(response => response.json());
                    }))
                        .then(pokemons => {
                            pokemons.forEach(pokemon => {
                                this.pokemons.push(pokemon);
                                if (this.favorites.includes(pokemon.id)) {
                                    pokemon.favorite = true;
                                }
                                if (this.team.includes(pokemon.id)) {
                                    pokemon.team = true;
                                }
                            });
                        });
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
                    pokemon.team = false;
                    localStorage.setItem('team', JSON.stringify(this.team));
                }
            } else {
                if (this.team.length < 6) {
                    pokemon.team = true;
                    this.team.push(pokemon.id);
                    localStorage.setItem('team', JSON.stringify(this.team));
                } else {
                    alert("Your Team Already Has 6 Pokemon");
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
    font-family: "Roboto", arial, sans-serif;
}

.container-fluid.header {
    display: flex;
    justify-content: center;
    align-items: center;
    background-color: #FFFFFF;
}

.pokemon-display {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    align-items: center;
    column-gap: 20px;
    row-gap: 50px;
    height: auto;
    background-color: #FFFFFF;
}

.filters {
    height: 90vh;
    background-color: #FFFFFF;
    position: sticky;
    top: 5vh;
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
    border-right: 1px solid gray;
    left: 0;
    gap: 25px;
}

.navbar {
    width: 100%;
    z-index: 100;
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

.dropdown-toggle {
    width: 155px;
    height: 50px;
    border-radius: 0px;
    background-color: #313131;
}

.dropdown-menu {
    max-height: 200px;
    overflow-y: auto;
    justify-content: center;
    align-items: center;
    flex-direction: column;
}

.filters-checks {
    gap: 20px;
}

.filters-checks input {
    margin-right: 5px;
}
</style>