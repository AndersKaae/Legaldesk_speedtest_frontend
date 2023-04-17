<script setup>
import { ref } from 'vue'
import UpOrDown from './components/UpOrDown.vue'
import Graph from './components/Graph.vue'
import axios from 'axios'

const loaded = ref(false);



var data_frontpage = {"pageName": "Front Page", "color": "rgb(52, 235, 88)", "data": []}
var data_productpage = {"pageName": "Product Page", "color": "rgb(52, 235, 88)", "data": []}
var data_wizard = {"pageName": "Front Page",  "color": "rgb(52, 235, 88)", "data": []}
var data_login = {"pageName": "Login", "color": "rgb(52, 235, 88)", "labels": [], "data": []}
var data_basket =  {"pageName": "Basket", "color": "rgb(52, 235, 88)", "labels": [], "data": []}

var performanceData = null
axios.get('https://pokeapi.co/api/v2/pokemon/')
  .then(response => {
    performanceData = response.data
    console.log(performanceData)
    data_frontpage["data"] = generateData()
    data_frontpage["labels"] = generateLabels()
    data_productpage["data"] = generateData()
    data_productpage["labels"] = generateLabels()
    data_wizard["data"] = generateData()
    data_wizard["labels"] = generateLabels()
    data_login["data"] = generateData()
    data_login["labels"] = generateLabels()
    data_basket["data"] = generateData()
    data_basket["labels"] = generateLabels()
    loaded.value = true
  })
  .catch(error => {
    console.error(error)
  })

  function generateData() {
  var data = []
  for (var i = 0; i < 300; i++) {
    data.push(Math.floor(Math.random() * 25))
  }
  return data
}

function generateLabels() {
  var labels = [];
  var startDate = new Date('April 1, 2023 00:00:00');
  for (var i = 0; i < 300; i++) {
    var date = new Date(startDate.getTime() + (i * 5 * 60 * 1000));
    labels.push(date);
  }
  return labels;
}

</script>

<template>
  <div v-if="loaded">
    <UpOrDown></UpOrDown>
    <Graph :PageData="data_frontpage"></Graph>
    <Graph :PageData="data_productpage"></Graph>
    <Graph :PageData="data_wizard"></Graph>
    <Graph :PageData="data_login"></Graph>
    <Graph :PageData="data_basket"></Graph>
  </div>
  <div v-else>
    <p>Loading...</p>
  </div>
</template>

<style scoped>
</style>
