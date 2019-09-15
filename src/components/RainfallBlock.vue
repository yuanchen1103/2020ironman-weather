<template>
  <div class="wrapper">
    <h1 class="title">
      Rainfall History
    </h1>
    <BarChart style="margin-top: 20px"
              :chartData="chartData" />
  </div>
</template>

<script>
import BarChart from './BarChart';

export default {
  props: ['chartData'],
  name: 'RainfallBlock',
  components: {
    BarChart
  }
};
</script>

<style scoped>
.wrapper {
  background: #ffffff;
  border-radius: 8px;
  padding: 20px 32px 15px 32px;
  width: fit-content;
}
.title {
  font-size: 16px;
  color: #5e565a;
  font-weight: 400;
  margin: 0;
}
</style>