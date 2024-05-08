<template>
    <div>
        <div class="type" v-for="type in types" :key="type.type.name" :style="pokemonTypeColor(type.type.name)">
            <h5>{{ type.type.name }}</h5>
        </div>
    </div>
</template>
<script>
export default {
    props: ['types'],
    methods: {
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
    }
}
</script>
<style scoped>

* {
    margin: 0;
    padding: 0;
    text-transform: capitalize;
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