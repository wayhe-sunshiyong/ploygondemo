<template>
  <div id="app">
    <ploygon></ploygon>

  </div>
</template>

<script>
// import HelloWorld from './components/HelloWorld.vue'
import Ploygon from './components/ploygon.vue'

export default {
  name: 'App',
  components: {
    Ploygon

    // HelloWorld
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
