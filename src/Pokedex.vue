<template>
    <Inventory></Inventory>
    <Shop></Shop>
    <Pokemons></Pokemons>
</template>

<script>
import Inventory from './components/Inventory.vue';
import Pokemons from './components/Pokemons.vue';
import Shop from './components/Shop.vue';

export default {
    components: {
        Pokemons, Inventory, Shop
    }
}
</script>

<style scoped>
* {
    margin: 0;
    padding: 0;
    text-transform: capitalize;
}
</style>