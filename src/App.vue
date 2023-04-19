<script setup>
import { ref } from 'vue'
import UpOrDown from './components/UpOrDown.vue'
import Graph from './components/Graph.vue'
import UptimePercentage from './components/UptimePercentage.vue'
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

axios.get('https://legaldeskspeedtest-production.up.railway.app/api/v1/get-days?records=288')
//axios.get('http://localhost:5000/api/v1/get-days?records=288')
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

function parseData(apiData) {
  // We are subtracting one from the length of the array because the last element might be incomplete because the test is running
  for(var i = 0; i < apiData['result'].length -1; i++) {
    var formattedTime = formatTime(apiData['result'][i]["datetime"])
    parsedData["frontPage"]["time"].push(formattedTime)
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

// Write a function to format a date to only include hours and minutes
function formatTime(time) {
  var time = new Date(time)
  var hours = time.getHours()
  var minutes = time.getMinutes()
  if (minutes < 10) {
    minutes = "0" + minutes
  }
  return hours + ":" + minutes
}

</script>

<template>
  <UpOrDown></UpOrDown>
  <div class="uptime-container">
    <UptimePercentage></UptimePercentage>
    <UptimePercentage></UptimePercentage>
    <UptimePercentage></UptimePercentage>
    <UptimePercentage></UptimePercentage>
    <UptimePercentage></UptimePercentage>
  </div>
  <div v-if="loaded">
    <Graph :PageData="data_frontpage"></Graph>
    <Graph :PageData="data_productpage"></Graph>
    <Graph :PageData="data_wizard"></Graph>
    <Graph :PageData="data_login"></Graph>
    <Graph :PageData="data_basket"></Graph>
  </div>
  <div id="loader" v-else>
    <img src="./assets/loader.gif" alt="Loading..."/>
  </div>
</template>

<style scoped>
#loader{
  padding-top: 5rem;
  display: block;
  margin-left: auto;
  margin-right: auto;
  width: 40%;
}

.uptime-container {
  margin: 0 auto;
  width: 70%;
  display: flex;
  flex-direction: row;
  padding-top: 2rem;
  padding-bottom: 4rem;
}
</style>
