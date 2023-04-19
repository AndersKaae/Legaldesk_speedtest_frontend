<script setup>

</script>

<template>
    <div class="container">
        <div class="title">Front Page</div>
        <div class="value">
            <span class="number">96.67</span>
            <span class="percent">%</span>
        </div>
    </div>
</template>

<style scoped>
    .container {
        border: 1px solid rgb(192, 192, 192);
        background-color: rgb(170, 255, 188);
        float:left;
        margin: 0 auto;
        width: 150px;
        height: 170px;
        /* rounded corners */
        -webkit-border-radius: 20px;
        /* drop shadow */
        -webkit-box-shadow: 0px 5px 10px rgba(0, 0, 0, 0.2);
    }
    .title {
        padding-top: 20px;
        text-align: center;
        font-size: 1.5rem;
    }

    .value {
        padding-top: 20px;
        text-align: center;
    }

    .number {
        font-size: 2rem;
    }
    .percent {
        font-size: 1.2rem;
    }
</style>
