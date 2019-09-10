<template>
  <div id="app">
    <LineChart />
  </div>
</template>

<script>
import LineChart from './components/LineChart';

export default {
  name: 'app',
  components: {
    LineChart
  }
}
</script>

<style>
@import './assets/css/normalize.css';
@import './assets/css/index.css';

#app {
  font-family: 'Lato', sans-serif;
}
</style>
