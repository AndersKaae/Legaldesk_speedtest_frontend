<template>
    <div class="chart-container" style="height:100px width:100%;">
    <Line
      id="my-chart-id"
      :options="chartOptions"
      :data="chartData"
    />
    </div>  
</template>
  
  <script>
  import { Line } from 'vue-chartjs'
  import { Chart as ChartJS, Title, Tooltip, Legend, CategoryScale, LinearScale, PointElement, LineElement } from 'chart.js'

  ChartJS.register(Title, Tooltip, Legend, CategoryScale, LinearScale, PointElement, LineElement)
  
  export default {
    props: {
        PageData: JSON
    },
    name: 'LineChart',
    components: { Line },
    data() {
      return {
        chartData: {
          labels: [ '1 april 00:00', '1 april 00:05', '1 april 00:10', '1 april 00:15', '1 april 00:20', '1 april 00:25', '1 april 00:30', '1 april 00:35', '1 april 00:40', '1 april 00:45', '1 april 00:50', '1 april 00:55', '1 april 01:00', '1 april 01:05','1 april 00:00', '1 april 00:05', '1 april 00:10', '1 april 00:15', '1 april 00:20', '1 april 00:25', '1 april 00:30', '1 april 00:35', '1 april 00:40', '1 april 00:45', '1 april 00:50', '1 april 00:55', '1 april 01:00', '1 april 01:05'],
          datasets: [ { 
            backgroundColor: this.PageData['color'],
            borderColor: this.PageData['color'],
            label: this.PageData['pageName'],
            data: this.PageData['data'] } ]
        },
        chartOptions: {
          width : 100,
          responsive: true,
          maintainAspectRatio: false,
          scales: {
            y: {
                beginAtZero: true,
                title: {
                    display: true,
                    text: 'Seconds'
                }
            }
          }

        }
      }
    },
  }
  </script>

<style scoped>
    .chart-container {
        height: 200px;
        width: 100%;
        padding-bottom: 2rem;
    }
</style>