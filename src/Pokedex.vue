<script>
import Pokemons from './components/Pokemons.vue';

export default {
    components: {
        Pokemons
    },
}
</script>

<template>
    <div id="pokemons">
        <Pokemons></Pokemons>
    </div>
</template>



<style scoped>
* {
    margin: 0;
    padding: 0;
    text-transform: capitalize;
}
</style>