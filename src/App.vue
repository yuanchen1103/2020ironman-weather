<template>
  <div id="app">
    <BarChart />
    <LineChart />
  </div>
</template>

<script>
import BarChart from './components/BarChart';
import LineChart from './components/LineChart';

export default {
  name: 'app',
  components: {
    BarChart,
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
