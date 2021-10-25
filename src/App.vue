<template>
  <div id="app">
    <the-header></the-header>
    <the-forecast></the-forecast>
  </div>
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator';
import TheHeader from "@/components/TheHeader.vue";
import TheForecast from "@/components/TheForecast.vue";

@Component({
  components: {
    TheHeader,
    TheForecast,
  },
})
export default class App extends Vue {}
</script>

<style lang="scss">
  @import '~materialize-css/sass/materialize';
</style>
