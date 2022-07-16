<template>
  <div class="graph-container">
    <canvas id="myChart" width="300" height="220" ref="Chart"></canvas>
  </div>
</template>

<script>
import {
  Chart,
  ArcElement,
  LineElement,
  BarElement,
  PointElement,
  BarController,
  BubbleController,
  DoughnutController,
  LineController,
  PieController,
  PolarAreaController,
  RadarController,
  ScatterController,
  CategoryScale,
  LinearScale,
  LogarithmicScale,
  RadialLinearScale,
  TimeScale,
  TimeSeriesScale,
  Decimation,
  Filler,
  Legend,
  Title,
  Tooltip,
  SubTitle
} from 'chart.js';

Chart.register(
    ArcElement,
    LineElement,
    BarElement,
    PointElement,
    BarController,
    BubbleController,
    DoughnutController,
    LineController,
    PieController,
    PolarAreaController,
    RadarController,
    ScatterController,
    CategoryScale,
    LinearScale,
    LogarithmicScale,
    RadialLinearScale,
    TimeScale,
    TimeSeriesScale,
    Decimation,
    Filler,
    Legend,
    Title,
    Tooltip,
    SubTitle
);

export default {
  name: "Graph",
  props:[
      'chanceOfRain','temperature',
  ],
  methods:{
    createGraph(){

      let labels = ['0h','1h','2h','3h','4h','5h','6h','7h','8h','9h','10h','11h','12h',
        '13h','14h','15h','16h','17h','18h','19h','20h','21h','22h','23h']
      let data = {
        labels: labels,
        datasets: [
          {
            label: 'Temperature',
            data: this.temperature,
            borderColor: 'red',
            fill: false,
            tension: 0.4
          }, {
            label: 'chance of rain',
            data: this.chanceOfRain,
            borderColor: 'blue',
            fill: false,
            tension: 0.4
          },
        ]
      };
      let config = {
        type: 'line',
        data: data,
        options: {
          responsive: true,
          aspectRatio: 1.5,
          onResize(chart, size) {
            chart.aspe
            console.log(size)
          },
          plugins: {
            title: {
              display: true,
              text: 'Chart.js Line Chart - Cubic interpolation mode'
            },
          },
          interaction: {
            intersect: false,
          },
          scales: {
            x: {
              display: true,
              title: {
                display: true
              }
            },
            y: {
              display: true,
              suggestedMin: 0,
              suggestedMax: 100
            }
          }
        },
      };
      let myChart = new Chart(this.$refs.Chart,config);
    }
  },
  watch: {
    temperature: function (){
      this.createGraph()
    }
  },
  mounted(){

  }
}
</script>

<style scoped>

</style>