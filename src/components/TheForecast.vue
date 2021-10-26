<template>
  <div class="fore-cast-container">
    <div class="current-info-wrapper">
      <current-info
          v-bind:main-info="mainInfo"
          v-bind:weather="weather"
      >
      </current-info>
    </div>
    <predictions
        v-bind:predictions-list="predictionsList"
        v-bind:isDataLoaded="isDataLoaded"
    >
    </predictions>
  </div>
</template>

<script lang="ts">
import {Component} from 'vue-property-decorator';
import CurrentInfo from "@/components/CurrentInfo.vue";
import MainInfoInterface from "@/interfaces/main-info.interface";
import WeatherInterface from "@/interfaces/weather.interface";
import Vue from "vue";
import axios from "axios";
import CityInterface from "@/interfaces/city.interface";
import Predictions from "@/components/Predictions.vue";
import Prediction from "@/interfaces/prediction.interface";

@Component({
  components: {
    Predictions,
    CurrentInfo
  }
})
export default class TheForecast extends Vue {
  isDataLoaded!: Promise<void>;


  mainInfo: MainInfoInterface = {
    temp: 0,
    feels_like: 0,
    pressure: 0,
    wind_speed: 0,
  }
  weather: WeatherInterface = {
    id: 0,
    icon: '',
    main: '',
    description: '',
  }
  city: CityInterface = {
    name: '',
    country: '',
    coord: {
      lat: 0.0,
      lon: 0.0
    }
  }
  predictionsList: Prediction[] = []

  mounted(): void {
    axios
        .get<any>('/data/2.5/forecast?id=524901&appid=8a39d3427f68c1be0169c949d64122bb&lang=ru', {
          baseURL: 'https://api.openweathermap.org'
        })
        .then(r => {
          this.city = {
            name: r.data.city.name,
            coord: r.data.city.coord,
            country: r.data.city.country
          }

          const current = r.data.list[0]

          this.mainInfo = {
            temp: current.main.temp,
            feels_like: current.main.feels_like,
            pressure: current.main.pressure,
            wind_speed: current.wind.speed,
          }

          this.weather = {
            id: current.weather[0].id,
            main: current.weather[0].main,
            icon: current.weather[0].icon,
            description: current.weather[0].description,
          }
          this.loadDaily()
        })

  }

  loadDaily(): void {
    axios.get<any>(`/data/2.5/onecall?lat=${this.city.coord.lat}&lon=${this.city.coord.lon}&exclude=current,minutely,hourly,alerts&appid=8a39d3427f68c1be0169c949d64122bb`, {
      baseURL: 'https://api.openweathermap.org'
    })
        .then(r => {
          // console.log(r.data)
          let index = 1
          this.predictionsList = r.data.daily.map((el: any) => {
            const pred: Prediction = {
              id: index++,
              date: new Date(el.dt * 1000),
              icon: el.weather[0].icon,
              min_temp: el.temp.min,
              max_temp: el.temp.max,
            }
            return pred
          })

          this.isDataLoaded.then()
          console.log(this.predictionsList)
        })

  }
}
</script>

<style lang="sass" scoped>
.fore-cast-container
  margin-top: 2em
  width: 100%
  display: flex
  justify-content: flex-start
  align-items: center
  flex-wrap: wrap
  padding: 2rem
</style>
