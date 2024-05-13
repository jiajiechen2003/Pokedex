<template>
    <div class="sliders">
        <input value="0" min="0" max="151" type="range" step="1" v-model="rangeValue1">
        <p>{{ rangeValue1 }}</p>
        <input value="151" min="0" max="151" type="range" step="1" v-model="rangeValue2" :disabled="rangeValue1 < rangeValue2">
        <p>{{ rangeValue2 }}</p>
    </div>
</template>

<script>
export default {
    data() {
        return {
            rangeValue1: 0,
            rangeValue2: 151
        }
    },
    watch: {
        rangeValue1() {
            this.$emit('slider', [this.rangeValue1, this.rangeValue2]);
        },
        rangeValue2() {
            this.$emit('slider', [this.rangeValue1, this.rangeValue2]);
        }
    }
}
</script>

<style scoped>
.sliders {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
}
</style>
