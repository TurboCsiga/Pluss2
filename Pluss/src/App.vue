<template>
  <div id="app">
    <Pluss/>
  </div>
</template>



<script>
  import Pluss from '../src/components/Pluss.vue'

  export default {
    name: 'App',
    components: {
      Pluss
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