<template>
  <div id="app">
    <grid></grid>
  </div>
</template>

<script>
import GridContainerComponent from '../src/components/grids/GridContainerComponent'

export default {
  name: 'App',
  components: {
    'grid': GridContainerComponent
  }
}
</script>

<style>
  @import '../node_modules/material-icons/iconfont/material-icons.css';
  @import '../node_modules/vuetify/dist/vuetify.min.css';
</style>
