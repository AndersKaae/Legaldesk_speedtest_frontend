<template>
  <div class="chart-container" style="height:100px width:100%;">
    <Line
      id="my-chart-id"
      :options="chartOptions"
      :data="computedChartData"
    />
  </div>
</template>

<script>
import { Line } from 'vue-chartjs';
import {
  Chart as ChartJS,
  Title,
  Tooltip,
  Legend,
  CategoryScale,
  LinearScale,
  PointElement,
  LineElement,
} from 'chart.js';

ChartJS.register(
  Title,
  Tooltip,
  Legend,
  CategoryScale,
  LinearScale,
  PointElement,
  LineElement
);

const down = (ctx, value) =>
  ctx.p0.parsed.y == 60 || ctx.p1.parsed.y == 60 ? value : undefined;
const missing = (ctx, value) =>
  ctx.p0.parsed.y == 0 || ctx.p1.parsed.y == 0 ? value : undefined;

export default {
  props: {
    PageData: Object,
    PageSection: String,
  },
  name: 'LineChart',
  components: { Line },
  computed: {
    computedChartData() {
      return {
        labels: this.PageData['labels'],
        datasets: [
          {
            backgroundColor: this.PageData['DK']['color'],
            borderColor: this.PageData['DK']['color'],
            label: 'DK ' + this.PageSection,
            data: this.PageData['DK'][this.PageSection]['data'],
            segment: {
              borderColor: (ctx) =>
                down(ctx, 'rgb(192,75,75)') || missing(ctx, 'rgb(0,0,0,0.1)'),
              backgroundColor: (ctx) => down(ctx, 'rgb(192,75,75)'),
            },
          },
          {
            backgroundColor: this.PageData['SE']['color'],
            borderColor: this.PageData['SE']['color'],
            label: 'SE ' + this.PageSection,
            data: this.PageData['SE'][this.PageSection]['data'],
            segment: {
              borderColor: (ctx) =>
                down(ctx, 'rgb(192,75,75)') || missing(ctx, 'rgb(0,0,0,0.1)'),
              backgroundColor: (ctx) => down(ctx, 'rgb(192,75,75)'),
            },
          },
          {
            backgroundColor: this.PageData['NO']['color'],
            borderColor: this.PageData['NO']['color'],
            label: 'NO ' + this.PageSection,
            data: this.PageData['NO'][this.PageSection]['data'],
            segment: {
              borderColor: (ctx) =>
                down(ctx, 'rgb(192,75,75)') || missing(ctx, 'rgb(0,0,0,0.1)'),
              backgroundColor: (ctx) => down(ctx, 'rgb(192,75,75)'),
            },
          }
        ],
      };
    },
  },
  data() {
    return {
      chartOptions: {
        width: 100,
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
              text: 'Seconds',
            },
          },
        },
      },
    };
  },
};
</script>

<style scoped>
    .chart-container {
        height: 200px;
        width: 100%;
        padding-bottom: 2rem;
    }
</style>