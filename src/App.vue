<template>
  <div id="app">
    <h3>Html5Preview.vue</h3>
    <Html5Preview msg="Some data..."/>
  </div>
</template>

<script>
import Html5Preview from './components/Html5Preview.vue'

export default {
  name: 'app',
  components: {
    Html5Preview
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
