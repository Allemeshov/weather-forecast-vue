<template>
  <div class="current-info-container z-depth-3">
    <div class="icon-wrapper">
      <img :src="iconUrl" alt="Weather icon">
    </div>
    <div class="temps-container">
      <h3>{{ mainInfo.temp | celsiusDegrees }}&#8451;</h3>
      <h6>Ощущается как {{ mainInfo.feels_like | celsiusDegrees }}&#8451;</h6>
    </div>
    <div class="description-container">
      <div class="weather-desc">
        <h4>{{ weather.description | camelCase }}</h4>
      </div>
      <p>Ветер: {{ mainInfo.wind_speed }} м/с</p>
      <p>Давление: {{ mainInfo.pressure }} мм рт.ст</p>
    </div>
  </div>
</template>

<script lang="ts">
import {Component, Prop, Vue} from 'vue-property-decorator';
import WeatherInterface from "@/interfaces/weather.interface";
import MainInfoInterface from "@/interfaces/main-info.interface";

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
export default class CurrentInfo extends Vue {
  @Prop() mainInfo!: MainInfoInterface
  @Prop() weather!: WeatherInterface


  public get iconUrl(): string {
    if (this.weather) {
      return 'https://openweathermap.org/img/wn/' + this.weather.icon + '@2x.png'
    }

    return ''
  }

  public set iconUrl(iconStr: string) {
    this.weather.icon = iconStr
  }
}
</script>

<style lang="sass" scoped>
h3
  text-align: center

.current-info-container
  max-width: 320px
  box-sizing: border-box
  display: flex
  justify-content: flex-start
  flex-wrap: wrap
  align-items: center

  .icon-wrapper
    padding: 0
    margin: 0

  .temps-container
    h3, h6
      margin: 0
    box-sizing: border-box !important
    padding: 1rem

  .description-container
    width: 100%
    display: flex
    flex-direction: column
    justify-content: center
    align-items: center

    box-sizing: border-box
    padding: 0 1rem 1rem

    p
      display: inline !important

    .weather-desc
      width: 100%
      text-align: center

</style>

