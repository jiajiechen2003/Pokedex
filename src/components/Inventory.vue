<template>
    <div class="modal fade" id="inventoryModal">
        <div class="modal-dialog modal-dialog-centered modal-dialog-scrollable">
            <div class="modal-content">
                <div class="modal-header">
                    <h1 class="modal-title fs-5">Inventory</h1>
                    <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                </div>
                <div class="modal-body">
                    <div class="row">
                        <div class="col-3" v-for="item in items">
                            <div class="card">
                                <div class="card-body">
                                    <img :src="item.sprites.default">
                                    <p>{{ item.name }}</p>
                                </div>

                            </div>
                        </div>
                    </div>
                </div>
                <!-- <div class="modal-footer">
                    <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Close</button>
                    <button type="button" class="btn btn-primary">Save changes</button>
                </div> -->
            </div>
        </div>
    </div>
</template>
<script>
export default {
    data() {
        return {
            items: []
        };
    },
    methods: {
        selectItems() {
            fetch('https://pokeapi.co/api/v2/item/')
                .then(response => response.json())
                .then(data => {
                    // Map through the fetched item URLs and fetch each item
                    Promise.all(data.results.map(item => fetch(item.url)))
                        .then(responses => Promise.all(responses.map(response => response.json())))
                        .then(items => {
                            this.items = items;
                        })
                        .catch(error => {
                            console.error('Error fetching item details:', error);
                        });
                })
                .catch(error => {
                    console.error('Error fetching item list:', error);
                });
        }
    },
    created() {
        this.selectItems();
    }
}
</script>
<style scoped>
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