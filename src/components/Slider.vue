<template>
    <div class="id-range">
        <input type="range" class="form-range" min="0" max="151" id="customRange1" v-model="rangeValue">
        <p>Valor del rango: {{ rangeValue }}</p>
    </div>
</template>
<script>
export default {
    props: {
        value: Number
    },
    data() {
        return {
            rangeValue: 0
        }
    },
    watch: {
        rangeValue(newValue) {
            this.$emit('slider', newValue)
        }
    }
}
</script>
<style scoped></style>