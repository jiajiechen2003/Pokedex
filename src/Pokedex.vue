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
    data(){
        return {
            items: []
        }
    },
    components: {
        Pokemons, Inventory, Shop
    },
    methods: {
        loadData(items) {
            this.items = items
            console.log(this.items)
        }
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