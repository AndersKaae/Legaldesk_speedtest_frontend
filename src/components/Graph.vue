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
          labels: this.PageData['labels'],
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
          borderWidth: 1,
          pointBorderWidth: 0,
          pointRadius: 1,
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