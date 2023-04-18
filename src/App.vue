<script setup>
import { ref } from 'vue'
import UpOrDown from './components/UpOrDown.vue'
import Graph from './components/Graph.vue'
import axios from 'axios'

const loaded = ref(false);



var data_frontpage = {"pageName": "Front Page", "color": "rgb(52, 235, 88)", "data": []}
var data_productpage = {"pageName": "Product Page", "color": "rgb(52, 235, 88)", "data": []}
var data_wizard = {"pageName": "Wizard",  "color": "rgb(52, 235, 88)", "data": []}
var data_login = {"pageName": "Login", "color": "rgb(52, 235, 88)", "labels": [], "data": []}
var data_basket =  {"pageName": "Basket", "color": "rgb(52, 235, 88)", "labels": [], "data": []}
var no_records = 300

var parsedData = {"frontPage":{"time":[],"data":[]},"productPage":{"time":[],"data":[]},"wizard":{"time":[],"data":[]},"login":{"time":[],"data":[]},"basket":{"time":[],"data":[]}}
var performanceData = null
axios.get('https://legaldeskspeedtest-production.up.railway.app/api/v1/get-days?days=1')
//axios.get('http://localhost:5000/api/v1/get-days?days=1')
  .then(response => {
    performanceData = response.data
    //console.log(performanceData)
    parsedData = parseData(performanceData)
    data_frontpage["labels"] = parsedData["frontPage"]["time"]
    data_frontpage["data"] = parsedData["frontPage"]["data"]
    data_productpage["labels"] = parsedData["productPage"]["time"]
    data_productpage["data"] = parsedData["productPage"]["data"]
    data_wizard["labels"] = parsedData["wizard"]["time"]
    data_wizard["data"] = parsedData["wizard"]["data"]
    data_login["labels"] = parsedData["login"]["time"]
    data_login["data"] = parsedData["login"]["data"]
    data_basket["labels"] = parsedData["basket"]["time"]
    data_basket["data"] = parsedData["basket"]["data"]
    loaded.value = true
  })
  .catch(error => {
    console.error(error)
  })

  function generateData() {
  var data = []
  for (var i = 0; i < no_records; i++) {
    data.push(Math.floor(Math.random() * 25))
  }
  return data
}

function generateLabels() {
  var labels = [];
  var startDate = new Date('April 1, 2023 00:00:00');
  for (var i = 0; i < no_records; i++) {
    var date = new Date(startDate.getTime() + (i * 5 * 60 * 1000));
    labels.push(date.toLocaleString());
  }
  return labels;
}

function parseData(apiData) {
  for(var i = 0; i < apiData['result'].length; i++) {
    parsedData["frontPage"]["time"].push(apiData['result'][i]["datetime"])
    parsedData["frontPage"]["data"].push(apiData['result'][i]["front_page"])
    parsedData["productPage"]["data"].push(apiData['result'][i]["product_page"])
    parsedData["wizard"]["data"].push(apiData['result'][i]["wizard"])
    parsedData["login"]["data"].push(apiData['result'][i]["login"])
    parsedData["basket"]["data"].push(apiData['result'][i]["basket"])
  }
  parsedData["productPage"]["time"] = parsedData["frontPage"]["time"]
  parsedData["wizard"]["time"] = parsedData["frontPage"]["time"]
  parsedData["login"]["time"] = parsedData["frontPage"]["time"]
  parsedData["basket"]["time"] = parsedData["frontPage"]["time"]
  return parsedData
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
