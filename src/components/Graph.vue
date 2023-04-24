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

  const down = (ctx, value) => ctx.p0.parsed.y == 60 || ctx.p1.parsed.y == 60 ? value : undefined;
  const missing = (ctx, value) => ctx.p0.parsed.y == 0 || ctx.p1.parsed.y == 0 ? value : undefined;

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
            data: this.PageData['data'],
            segment: {
            borderColor: ctx => down(ctx, 'rgb(192,75,75)') || missing(ctx, 'rgb(0,0,0,0.1)'),
            backgroundColor: ctx => down(ctx, 'rgb(192,75,75)')
      }
          } ]
        },
        chartOptions: {
          width : 100,
          responsive: true,
          maintainAspectRatio: false,
          borderWidth: 2,
          pointBorderWidth: 0,
          pointRadius: 1.5,
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