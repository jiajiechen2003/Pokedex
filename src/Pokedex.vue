<template>
    <Header></Header>
    <Pokemons></Pokemons>
</template>

<script>
import Pokemons from './components/Pokemons.vue';
import Header from './components/Header.vue';

export default {
    components: {
        Pokemons,
        Header
    },
}
</script>

<style scoped>
* {
    margin: 0;
    padding: 0;
    text-transform: capitalize;
}
</style>