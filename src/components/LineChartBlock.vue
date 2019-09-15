<template>
  <div class="wrapper">
    <div class="block-top">
      <div style="margin-left: 32px;">
        <h1 class="avg-data"
            :style="{color}">
          {{avg}}
        </h1>
        <p class="title">{{title}}</p>
      </div>
      <div class="icon"
           :style="{background: convertHex(color)}">
        <img :src="icon"
             alt="">
      </div>
    </div>
    <LineChart :color="color"
               style="margin-bottom: 25px; margin-top: 63px;"
               :chartData="chartData" />
  </div>
</template>

<script>
import LineChart from './LineChart';

export default {
  props: ['title', 'color', 'icon', 'chartData', 'avg'],
  name: 'LineChartBlock',
  components: {
    LineChart
  },
  methods: {
    convertHex(color) {
      if (color.substring(0, 1) == '#') {
        color = color.substring(1);
      }

      /* Grab each pair (channel) of hex values and parse them to ints using hexadecimal decoding */
      const r = parseInt(color.substring(0, 2), 16);
      const g = parseInt(color.substring(2, 4), 16);
      const b = parseInt(color.substring(4), 16);
      return `rgba(${r}, ${g}, ${b}, 0.2)`;
    }
  }
};
</script>

<style scoped>
.wrapper {
  background: #ffffff;
  border-radius: 8px;
  width: 260px;
}

.block-top {
  margin-top: 23px;
  display: flex;
}

.avg-data {
  font-size: 32px;
  font-weight: 400;
  margin: 0;
}

.title {
  margin: 0;
  font-size: 12px;
  color: #9b9b9b;
  font-weight: 400;
}

.icon {
  height: 53px;
  width: 53px;
  border-radius: 50%;
  margin-left: 80px;
  display: flex;
  align-items: center;
  justify-content: center;
}
</style>