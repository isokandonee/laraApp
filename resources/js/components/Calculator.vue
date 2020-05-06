<template>
    <div class="calculator">
        <div class="display">0.012345</div>
        <div class="btn">C</div>
        <div class="btn">+/-</div>
        <div class="btn">%</div>
        <div class="btn">/</div>
        <div class="btn">7</div>
        <div class="btn">8</div>
        <div class="btn">9</div>
        <div class="btn">x</div>
        <div class="btn">4</div>
        <div class="btn">5</div>
        <div class="btn">6</div>
        <div class="btn">-</div>
        <div class="btn">1</div>
        <div class="btn">2</div>
        <div class="btn">3</div>
        <div class="btn">+</div>
        <div class="zero">0</div>
        <div class="btn">.</div>
        <div class="btn">=</div>
    </div>
</template>

<script>
    export default {
        mounted() {
            console.log('Component mounted.')
        }
    }
</script>

<style scoped>
    .calculator {
        font-size: 40px;
        display:grid;
        grid-template-columns: repeat(4, 1fr);
        grid-auto-rows: mimax(50px, auto);
    }

    .display{
        grid-column: 1 / 5;
    }

    .zero{
        grid-column: 1 / 3;
    }

    .btn{
        font-size: 30px;
        background-color: #eee;
        border: 1px solid #333;
    }

    .operator{
        background-color: orange;
        
    }
</style>