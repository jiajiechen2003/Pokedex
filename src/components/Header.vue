<template>
    <nav class="navbar navbar-expand-lg bg-body-tertiary">
        <div class="container-fluid">
            <a class="navbar-brand" href="#"><img src="../img/pokemon-logo.png"></a>
            <div class="dropdown">
                <button class="btn btn-secondary dropdown-toggle" type="button" data-bs-toggle="dropdown"
                    aria-expanded="false">
                    Types
                </button>
                <ul class="dropdown-menu">
                    <li><a class="dropdown-item">Grass</a></li>
                    <li><a class="dropdown-item">Poison</a></li>
                    <li><a class="dropdown-item">Fire</a></li>
                    <li><a class="dropdown-item">Water</a></li>
                    <li><a class="dropdown-item">Bug</a></li>
                    <li><a class="dropdown-item">Flying</a></li>
                    <li><a class="dropdown-item">Normal</a></li>
                    <li><a class="dropdown-item">Electric</a></li>
                    <li><a class="dropdown-item">Ground</a></li>
                    <li><a class="dropdown-item">Fairy</a></li>
                    <li><a class="dropdown-item">Fighting</a></li>
                    <li><a class="dropdown-item">Psychic</a></li>
                    <li><a class="dropdown-item">Rock</a></li>
                    <li><a class="dropdown-item">Steel</a></li>
                    <li><a class="dropdown-item">Ice</a></li>
                    <li><a class="dropdown-item">Ghost</a></li>
                    <li><a class="dropdown-item">Dragon</a></li>
                </ul>
            </div>
            
        </div>

    </nav>
</template>
<script>
export default {

}
</script>
<style scoped>
.container-fluid {
    display: flex;
    justify-content: center;
    align-items: center;
}
</style>