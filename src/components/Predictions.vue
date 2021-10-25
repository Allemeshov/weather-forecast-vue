<template>
  <div class="predictions-container">
    <div class="week">
      <div class="date" v-for="pred in predictionsList" :key="pred.id">
        <p class="day">{{pred.date.getDay()}}</p>
        <p class="date">{{pred.date.getDate()}}</p>
        <img :src="iconUrl(pred)" alt="Weather icon">
      </div>
    </div>
    <div class="graph"></div>
  </div>
</template>

<script lang="ts">
import {Component, Prop, Vue} from 'vue-property-decorator';
import Prediction from "@/interfaces/prediction.interface";

@Component({
  filters: {
    celsiusDegrees(degree: number): number {
      return Math.round(degree - 273.15)
    },
    camelCase(str: string): string {
      return str[0].toUpperCase() + str.slice(1)
    }
  }
})
export default class Predictions extends Vue{
  @Prop() predictionsList!: Prediction[]

  public iconUrl(prediction: Prediction): string {
    if (prediction) {
      return 'https://openweathermap.org/img/wn/' + prediction.icon + '@2x.png'
    }

    return ''
  }
}
</script>

<style lang="sass" scoped>
.predictions-container
  min-width: 400px
  min-height: 248px
  border: 1px solid black
  margin-left: 3em
  box-sizing: border-box

  .week
    width: 100%
    height: 35%
    background-color: #ccc
    display: flex
    justify-content: flex-start
    align-items: center
</style>
