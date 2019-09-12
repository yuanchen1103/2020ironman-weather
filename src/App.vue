<template>
  <div id="app">
    <BarChart />
  </div>
</template>

<script>
import BarChart from './components/BarChart';

export default {
  name: 'app',
  components: {
    BarChart
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
