<template>
  <canvas id="graph"></canvas>
</template>

<script lang="ts">
import {Component, Prop, Vue} from 'vue-property-decorator';
import {Chart, ChartConfiguration, ChartData, registerables} from "chart.js";

Chart.register(...registerables);

@Component
export default class Graph extends Vue {
  @Prop({default: []}) readonly minValues!: Array<number>
  @Prop({default: []}) readonly maxValues!: Array<number>
  @Prop({default: []}) readonly labels!: Array<string>
  @Prop() readonly isDataLoaded!: Promise<void>

  mounted(): void {
    setTimeout(() => {
      this.createChart({
        labels: this.labels,
        datasets: [
          {
            label: 'Максимальная температура',
            data: this.maxValues,
            fill: 'origin',
            backgroundColor: 'rgba(255, 99, 132, 0.2)'
          },
          {
            label: 'Минимальная температура',
            data: this.minValues,
            fill: 'origin',
            backgroundColor: 'rgba(75, 192, 192, 0.2)'
          }
        ]
      })
    }, 2000)

  }

  createChart(chartData: ChartData): Chart {
    const canvas = document.getElementById('graph') as HTMLCanvasElement
    const options: ChartConfiguration = {
      type: "line",
      data: chartData
    }
    return new Chart(canvas, options)
  }
}
</script>

<style lang="sass" scoped>
canvas
  width: 100%
  height: 100%
</style>
