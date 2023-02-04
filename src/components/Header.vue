<script setup lang="ts">

</script>

<template>
    <h1>
        <span>JAN</span>
        <span>KEN</span>
        <span>PON</span>
    </h1>
</template>

<style scoped>
    h1 {
        overflow: hidden;
        font-family: 'Unbounded', cursive;
        display: flex;
        justify-content: center;
        background-color: #FAFAFB;
        box-shadow: 0 0 12px 2px rgba(75, 82, 122, .5);
        padding: 20px 25px;
        font-size: 3em;
        width: max-content;
        margin: 15px auto;
        border-radius: 80px;
    }

    h1 span {
        display: flex;
        justify-content: center;
        padding: 10px 25px;
        border-radius: 60px;
    }

    span:nth-child(1) {
        color: #464D77;
        background-color: #EF3D61;
    }
    span:nth-child(2) {
        color: white;
        background-color: #565C82;
    }
    span:nth-child(3) {
        color: #4B527A;
        background-color: #F6E56C;
    }
</style>