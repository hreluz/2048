<template>
  <div id="app">
    <app-grid></app-grid>
  </div>
</template>

<script>
import Grid from './Grid.vue'
export default {
  components:{
    'app-grid' : Grid
  },
  name: 'app',
}
</script>
