<template>
    <Shop @itemsData="loadData"></Shop>
    <div class="modal fade" id="inventoryModal">
        <div class="modal-dialog modal-dialog-centered modal-dialog-scrollable">
            <div class="modal-content">
                <div class="modal-header">
                    <h1 class="modal-title fs-5">Inventory</h1>
                    <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                </div>
                <div class="modal-body">
                    <div class="row">
                        <div class="col-3" v-for="item in itemsWithQuantity">
                            <div class="card">
                                <div class="card-body">
                                    <img :src="item.sprites.default">
                                    <p>{{ item.name + ' x' + item.quantity }}</p>
                                </div>

                            </div>
                        </div>
                    </div>
                </div>
                <div class="modal-footer">
                    <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Close</button>
                </div>
            </div>
        </div>
    </div>
</template>
<script>
import Shop from './Shop.vue';

export default {
    components: {
        Shop
    },
    computed: {
        itemsWithQuantity() {
            return this.items.filter(item => item.quantity > 0)
        }
    },
    data() {
        return {
            items: []
        };
    },
    methods: {
        loadData(items) {
            this.items = items
            console.log(this.items)
        }   
    },
    created() {
    }
}
</script>
<style scoped>
* {
    text-transform: capitalize;
}
.btn {
    width: 100px;
    height: 40px;
    border-radius: 0px;
}

.modal-dialog {
    max-width: none;
    max-height: none;
    justify-content: center;
}

.modal-content {
    width: 80%;
    height: 90vh;
}

.row {
    gap: 30px;
    display: flex;
    justify-content: center;
    align-items: center;
    flex-wrap: wrap;
}

.card {
    width: 250px;
    height: 250px;
}

.card-body {
    padding: 0;
    display: flex;
    align-items: center;
    flex-direction: column;
}

.card-body img {
    width: 250px;
    height: 250px;
    background-color: #F2F2F2;
}
</style>