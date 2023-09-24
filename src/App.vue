<template>
  <div id="app">
    <CircleProgress :maxValue="108" :value="35" :targetValue="80"/>
  </div>
</template>

<script>
import CircleProgress from './components/CircleProgress'
export default {
  name: 'App',
  components: {
    CircleProgress
  }
}
</script>

<style lang="scss">

* {
  box-sizing: border-box;
  padding: 0;
  margin: 0;
}

#app {
  display: flex;
  align-items: center;
  justify-content: center;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  background: rgba(60, 60, 62, 0.8);
  height: 100vh;
}

h2, h4 {
  padding: 0;
  margin: 0;
}
</style>
