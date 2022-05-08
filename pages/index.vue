<template>
    <div class="container-4">
        <div class="list"></div>
        <div class="list"></div>
        <div class="list"></div>
        <div class="list"></div>
        <div class="list"></div>
        <div class="list"></div>
        <div class="list"></div>
    </div>
</template>


<style lang="scss">
.container-4 {
    width: 400px;
    display: grid;
    justify-content: center;
    grid-template-columns: auto auto;
    grid-gap: 10px;
    .list {
        width: 100px;
        height: 100px;
        background-color: skyblue;
    }
}
</style>