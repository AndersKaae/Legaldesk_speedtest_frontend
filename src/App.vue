<script setup>
import { ref, reactive } from 'vue'
import UpOrDown from './components/UpOrDown.vue'
import Graph from './components/Graph.vue'
import UptimePercentage from './components/UptimePercentage.vue'
import axios from 'axios'

const loaded = ref(false);

var records = ref(288)
const isMobile = navigator.userAgentData.mobile;
if (isMobile == true)
    {
      records.value = 50;
    }

var data_object = reactive({"DK":{"color":"rgb(75,192,192)","Frontpage":{"data":[],"up":null},"ProductPage":{"data":[],"up":null},"Wizard":{"data":[],"up":null},"Login":{"data":[],"up":null},"Basket":{"data":[],"up":null}},"SE":{"color":"rgb(255,99,132)","Frontpage":{"data":[],"up":null},"ProductPage":{"data":[],"up":null},"Wizard":{"data":[],"up":null},"Login":{"data":[],"up":null},"Basket":{"data":[],"up":null}},"NO":{"color":"rgb(255,159,64)","Frontpage":{"data":[],"up":null},"ProductPage":{"data":[],"up":null},"Wizard":{"data":[],"up":null},"Login":{"data":[],"up":null},"Basket":{"data":[],"up":null}}})



var performanceData = null

const apiUrl = 'https://legaldeskspeedtest-production.up.railway.app/'
//const apiUrl = 'http://localhost:5000/'

function upTrueFalse(page){
  var page_data = page["data"]
  if (page_data[page_data.length - 1] == 60){
        return false
      } else {
        return true
      }
}

function getGraphData(country, data_object){
  axios.get(`${apiUrl}api/v1/get-days?records=${records.value}&country=${country}`)
    .then(response => {
      performanceData = response.data
      var apiData = parseData(performanceData)

      // Setttings the labels:
      data_object["labels"] = apiData["frontPage"]["time"]
      // Frontpage
      data_object[country]["Frontpage"]["data"] = apiData["frontPage"]["data"]
      data_object[country]["Frontpage"]["up"] = upTrueFalse(data_object[country]["Frontpage"])
      // ProductPage
      data_object[country]["ProductPage"]["data"] = apiData["productPage"]["data"]
      data_object[country]["ProductPage"]["up"] = upTrueFalse(data_object[country]["ProductPage"])
      // Wizard
      data_object[country]["Wizard"]["data"] = apiData["wizard"]["data"]
      data_object[country]["Wizard"]["up"] = upTrueFalse(data_object[country]["Wizard"])
      // Login
      data_object[country]["Login"]["data"] = apiData["login"]["data"]
      data_object[country]["Login"]["up"] = upTrueFalse(data_object[country]["Login"])
      // Basket
      data_object[country]["Basket"]["data"] = apiData["basket"]["data"]
      data_object[country]["Basket"]["up"] = upTrueFalse(data_object[country]["Basket"])
      loaded.value = true
    })
    .catch(error => {
      console.error(error)
  })
  console.log(data_object)
}

getGraphData("DK", data_object)
getGraphData("SE", data_object)
getGraphData("NO", data_object)

function parseData(apiData) {
  var parsedData = {"frontPage":{"time":[],"data":[]},"productPage":{"time":[],"data":[]},"wizard":{"time":[],"data":[]},"login":{"time":[],"data":[]},"basket":{"time":[],"data":[]}}
  // We are subtracting one from the length of the array because the last element might be incomplete because the test is running
  for(var i = 0; i < apiData['result'].length; i++) {
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
    <UptimePercentage :PageSection = "'front_page'" :Up = "data_object['DK']['Frontpage']['up']"></UptimePercentage>
    <UptimePercentage :PageSection = "'product_page'" :Up = "data_object['DK']['ProductPage']['up']"></UptimePercentage>
    <UptimePercentage :PageSection = "'wizard'" :Up = "data_object['DK']['Wizard']['up']"></UptimePercentage>
    <UptimePercentage :PageSection = "'login'" :Up = "data_object['DK']['Login']['up']"></UptimePercentage>
    <UptimePercentage :PageSection = "'basket'" :Up = "data_object['DK']['Basket']['up']"></UptimePercentage>
  </div>
  <div class="graph-container">
    <Graph :PageSection = "'Frontpage'" :PageData="data_object"></Graph>
    <Graph :PageSection = "'ProductPage'" :PageData="data_object"></Graph>
    <Graph :PageSection = "'Wizard'" :PageData="data_object"></Graph>
    <Graph :PageSection = "'Login'" :PageData="data_object"></Graph>
    <Graph :PageSection = "'Basket'" :PageData="data_object"></Graph>
  </div>
</template>

<style scoped>
#loader{
  widows: 100px;
  padding-top: 5rem;
  display: block;
  display: flex;
  justify-content: center;
}

.uptime-container {
  margin: 0 auto;
  width: 70%;
  display: flex;
  flex-direction: row;
  padding-top: 2rem;
  padding-bottom: 4rem;
}

@media screen and (max-width: 1200px) {
  .uptime-container {
    flex-direction: column;
    align-items: center;
    width: 100%;
  }
}

.graph-container{
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
</style>
