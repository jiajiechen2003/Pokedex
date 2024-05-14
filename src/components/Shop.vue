<template>
    <div class="modal fade" id="shopModal">
        <div class="modal-dialog modal-dialog-centered modal-dialog-scrollable">
            <div class="modal-content">
                <div class="modal-header">
                    <h1 class="modal-title fs-5">Shop</h1>
                    <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                </div>
                <div class="modal-body">
                    <div class="row">
                        <div class="col-3" v-for="(item, index) in items">
                            <div class="card">
                                <div class="card-body">
                                    <img :src="item.sprites.default">
                                    <div class="change-quantity">
                                        <button @click="decrement(index)">-</button>
                                        <p>{{ item.addItems }}</p>
                                        <button @click="increment(index)">+</button>
                                    </div>
                                </div>

                            </div>
                        </div>
                    </div>
                </div>
                <div class="modal-footer">
                    <button type="button" class="btn btn-primary" data-bs-dismiss="modal" @click="buyItems">Buy</button>
                </div>
            </div>
        </div>
    </div>
</template>
<script>
export default {
    data() {
        return {
            items: [],
        };
    },
    methods: {
        selectItems() {
            fetch('https://pokeapi.co/api/v2/item/')
                .then(response => response.json())
                .then(data => {
                    Promise.all(data.results.map(item => fetch(item.url)))
                        .then(responses => Promise.all(responses.map(response => response.json())))
                        .then(items => {
                            const itemData = JSON.parse(localStorage.getItem('items'));
                            this.items = items.map((item, index) => ({ ...item, quantity: itemData[index].quantity || 0, addItems: 0 }));
                            console.log(this.items);
                        })
                        .catch(error => {
                            console.error('Error fetching item details:', error);
                        });
                })
                .catch(error => {
                    console.error('Error fetching item list:', error);
                });
        },
        increment(index) {
            this.items[index].addItems++;
        },
        decrement(index) {
            if (this.items[index].addItems > 0) {
                this.items[index].addItems--;
            }
        },
        buyItems() {
            this.items.forEach(item => {
                item.quantity += item.addItems;
                item.addItems = 0;
            });
            localStorage.setItem('items', JSON.stringify(this.items))
            console.log(this.items)
        },
    },
    created() {
        this.selectItems();
    },
    watch: {
        items() {
            this.$emit('itemsData', this.items)
        }
    }
}
</script>
<style scoped>
* {
    margin: 0;
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
    gap: 65px;
    display: flex;
    justify-content: center;
    align-items: center;
    flex-wrap: wrap;
    margin-bottom: 60px;
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
    border: 1px solid black;
}

.card-body img {
    width: 100%;
    background-color: #F2F2F2;
}

.change-quantity {
    display: flex;
    justify-content: center;
    align-items: center;
    gap: 25px;
    height: 40px;
}
</style>