<template>
<header>
  <h1>Welcome to Neverain forecast</h1>
</header>
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator';

@Component
export default class TheHeader extends Vue{
}
</script>

<style lang="sass" scoped>
header
  width: 100%
  min-height: 10vh
  border-bottom: 2px solid #111
  h1
    padding: 0
    margin: 0
</style>
