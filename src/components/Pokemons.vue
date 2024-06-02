<template>
    <div class="container-fluid">
        <div class="row">
            <div class="col-2 filters">
                <div class="dropdown">
                    <button class="btn btn-secondary dropdown-toggle" type="button" data-bs-toggle="dropdown"
                        aria-expanded="false">
                        Filter by Type
                    </button>
                    <ul class="dropdown-menu">
                        <li v-for="type in pokemonTypes" @click="selectType(type)">
                            <a class="dropdown-item">{{ type }}</a>
                        </li>
                    </ul>
                </div>
                <div class="filters-checks">
                    <div class="team-check">
                        <input type="checkbox" id="onTeam" v-model="teamPokemons">
                        <label for="onTeam">Team</label>
                    </div>
                    <div class="favorites-check">
                        <input type="checkbox" id="onFavorites" v-model="favoritePokemons">
                        <label for="onFavorites">Favorites</label>
                    </div>

                    <Slider @slider="sliderRange"></Slider>
                </div>

                <button type="button" class="btn btn-warning" data-bs-toggle="modal"
                    data-bs-target="#inventoryModal">Inventory</button>

                <button type="button" class="btn btn-warning" data-bs-toggle="modal"
                    data-bs-target="#shopModal">Shop</button>
            </div>
            <div class="col-10 pokemon-display">
                <nav class="navbar navbar-expand-lg bg-body-tertiary">
                    <div class="container-fluid header">
                        <img src="../img/pokedex-logo.png">
                    </div>
                </nav>
                <div class="card-col" v-for="pokemon in filter">
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
</template>
<script>
import Types from './Types.vue';
import Slider from './Slider.vue';
import Inventory from './Inventory.vue';
import Shop from './Shop.vue';

export default {
    components: {
        Types, Slider, Inventory, Shop
    },
    data() {
        return {
            pokemons: [],
            favorites: [],
            team: [],
            teamPokemons: false,
            favoritePokemons: false,
            pokemonTypes: ["all", "grass", "poison", "fire", "water", "bug", "flying", "normal", "electric", "ground", "fairy", "fighting", "psychic", "rock", "steel", "ice", "ghost", "dragon"],
            type: null,
            rangeValue1: 0,
            rangeValue2: 0,
        };
    },
    computed: {
        filter() {
            if (this.teamPokemons) {
                // if (this.team.length !== 6) {
                // return this.pokemons;
                // alert("You need 6 Pokemons on your team")
                // } else {
                return this.pokemons.filter(pokemon => this.team.includes(pokemon.id));
                // }
            } else if (this.favoritePokemons) {
                return this.pokemons.filter(pokemon => this.favorites.includes(pokemon.id));
            } else if (this.type) {
                if (this.type === "all") {
                    this.type = null;
                    return this.pokemons
                } else {
                    return this.pokemons.filter(pokemon => pokemon.types.some(type => type.type.name === this.type));
                }
            } else if (this.rangeValue1 > 0) {
                return this.pokemons.filter(pokemon => pokemon.id >= this.rangeValue1 && pokemon.id <= this.rangeValue2);
            }
            else {
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
                            { }
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
                    pokemon.team = false;
                    this.team.splice(index, 1);
                }
            } else {
                if (this.team.length < 6) {
                    pokemon.team = true;
                    this.team.push(pokemon.id);
                } else {
                    alert("Your Team Already Has 6 Pokemon");
                }
            }
            localStorage.setItem('team', JSON.stringify(this.team));
        },
        loadTeam() {
            this.team = JSON.parse(localStorage.getItem('team')) || [];
        },
        selectType(type) {
            this.type = type;
            console.log(type);
            console.log(this.type)
        },
        sliderRange(value1, value2) {
            this.rangeValue1 = value1;
            this.rangeValue2 = value2;
            console.log(this.rangeValue1)
        },
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
    background-color: #4152D6;
}

.dropdown-menu {
    max-height: 200px;
    overflow-y: auto;
    justify-content: center;
    align-items: center;
    flex-direction: column;
}

.filters-checks {
    gap: 5px;
    display: flex;
    /* justify-content: center; */
    flex-direction: column;
    /* align-items: center; */
}

.filters-checks input {
    margin-right: 5px;
}

.btn-warning {
    width: 100px;
    height: 40px;
    border-radius: 0px;
}
</style>