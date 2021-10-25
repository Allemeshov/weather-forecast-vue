<template>
  <div class="current-info-container">
    <div class="icon-wrapper">
      <img :src="iconUrl" alt="Weather icon">
    </div>
  </div>
</template>

<script lang="ts">
import {Component, Prop, Vue} from 'vue-property-decorator';
import WeatherInterface from "@/interfaces/weather.interface";
import MainInfoInterface from "@/interfaces/main-info.interface";

@Component
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

</style>
