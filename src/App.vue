<script setup>
import { ref } from 'vue'
import UpOrDown from './components/UpOrDown.vue'
import Graph from './components/Graph.vue'
import axios from 'axios'

const loaded = ref(false);

var n_frontpage = "Front Page"
var n_productpage = "Product Page"
var n_frontpage = "Frontpage"
var n_wizard = "Wizard"
var n_login = "Login"
var n_basket = "Basket"

var performanceData = null
axios.get('https://pokeapi.co/api/v2/pokemon/')
  .then(response => {
    performanceData = response.data
    console.log(performanceData)
    loaded.value = true
  })
  .catch(error => {
    console.error(error)
  })

</script>

<template>
  <div v-if="loaded">
    <UpOrDown></UpOrDown>
    <Graph :PageType="n_frontpage"></Graph>
    <Graph :PageType="n_productpage"></Graph>
    <Graph :PageType="n_wizard"></Graph>
    <Graph :PageType="n_login"></Graph>
    <Graph :PageType="n_basket"></Graph>
  </div>
  <div v-else>
    <p>Loading...</p>
  </div>
</template>

<style scoped>
</style>
