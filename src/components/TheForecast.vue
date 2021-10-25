<template>
  <div class="fore-cast-container">
    <div class="current-info-wrapper">
      <current-info
          v-bind:main-info="mainInfo"
          v-bind:weather="weather"
      >
      </current-info>
    </div>
  </div>
</template>

<script lang="ts">
import {Component, Prop} from 'vue-property-decorator';
import CurrentInfo from "@/components/CurrentInfo.vue";
import MainInfoInterface from "@/interfaces/main-info.interface";
import WeatherInterface from "@/interfaces/weather.interface";
import Vue from "vue";
import axios from "axios";

@Component({
  components: {
    CurrentInfo
  }
})
export default class TheForecast extends Vue {
  @Prop() mainInfo!: MainInfoInterface
  @Prop() weather!: WeatherInterface

  mounted(): void {
    axios
        .get<any>('/data/2.5/forecast?id=524901&appid=8a39d3427f68c1be0169c949d64122bb', {
          baseURL: 'https://api.openweathermap.org'
        })
        .then(Response => {
          console.log(Response.data.city)
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

  .current-info-container
    height: 100px
</style>
