<template>
    <div>
        <div v-if="pokemons.length > 0">
            <h2>Lista de nombres de Pokémon:</h2>
            <ul>
                <li v-for="pokemon in pokemons" :key="pokemon.name">
                    <strong>{{ pokemon.name }}</strong> - Habilidades:
                    <ul>
                        <li v-for="ability in pokemon.abilities" :key="ability.ability.name">{{ ability.ability.name }}</li>
                    </ul>
                </li>
            </ul>
        </div>
    </div>
</template>

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

<style scoped>
/* Estilos específicos del componente */
</style>