<template>
  <div class="predictions-container">
    <div class="week">
      <div class="date" v-for="pred in predictionsList" :key="pred.id">
        <p class="day">{{ pred.date.getDay() | dayString }}</p>
        <p class="date">{{ getShortDate(pred.date) }}</p>
        <img :src="iconUrl(pred)" alt="Weather icon">
      </div>
    </div>
    <div class="graph-wrapper">
      <graph
          v-bind:max-values="getMaxValues()"
          v-bind:min-values="getMinValues()"
          v-bind:labels="getLabels()"
          v-bind:is-data-loaded="isDataLoaded"
      />
    </div>
  </div>
</template>

<script lang="ts">
import {Component, Prop, Vue} from 'vue-property-decorator';
import Prediction from "@/interfaces/prediction.interface";
import Graph from "@/components/Graph.vue";

@Component({
  components: {Graph},
  filters: {
    celsiusDegrees(degree: number): number {
      return Math.round(degree - 273.15)
    },
    camelCase(str: string): string {
      if (!str)
        return ''

      return str[0].toUpperCase() + str.slice(1)
    },
    dayString(day: number): string {
      let res: string
      switch (day) {

        case 0:
          res = 'Вс'
          break
        case 1:
          res = 'Пн'
          break
        case 2:
          res = 'Вт'
          break
        case 3:
          res = 'Ср'
          break
        case 4:
          res = 'Чт'
          break
        case 5:
          res = 'Пт'
          break
        case 6:
          res = 'Сб'
          break
        default:
          res = 'unknown'
          break;
      }

      return res
    }
  }
})
export default class Predictions extends Vue {
  @Prop() predictionsList!: Prediction[]
  @Prop() isDataLoaded!: Promise<void>

  public iconUrl(prediction: Prediction): string {
    if (prediction) {
      return 'https://openweathermap.org/img/wn/' + prediction.icon + '@2x.png'
    }

    return ''
  }

  public getShortDate(date: Date): string {
    if (!date)
      return ''

    const dd = date.getDate()
    let mon: string
    switch (date.getMonth()) {
      case 0:
        mon = "Янв"
        break;
      case 1:
        mon = "Фев"
        break;
      case 2:
        mon = "Мар"
        break;
      case 3:
        mon = "Апр"
        break;
      case 4:
        mon = "Май"
        break;
      case 5:
        mon = "Июн"
        break;
      case 6:
        mon = "Июл"
        break;
      case 7:
        mon = "Авг"
        break;
      case 8:
        mon = "Сен"
        break;
      case 9:
        mon = "Окт"
        break;
      case 10:
        mon = "Дек"
        break;
      case 11:
        mon = "Ноя"
        break;
      default:
        mon = "Unknown"
        break;
    }

    return dd.toString() + ' ' + mon
  }

  celsiusDegrees(far: number): number {
    return Math.round(far - 273.15)
  }

  getMinValues(): number[] {
    return this.predictionsList.map(p => {
      return this.celsiusDegrees(p.min_temp)
    })
  }

  getMaxValues(): number[] {
    return this.predictionsList.map(p => {
      return this.celsiusDegrees(p.max_temp)
    })
  }

  getLabels(): string[] {
    return this.predictionsList.map(p => {
      return this.getShortDate(p.date)
    })
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

    text-align: center

    .graph-wrapper
      width: 300px
      height: 100px

</style>
