<template>
  <div id="app">
    <div class="main-container">
      <v-select v-model="selected"
                :options="['Taipei']"
                :clearable="false"
                :searchable="false"
                style="width: 201px;"></v-select>
      <div class="row" style="margin-top: 28px;">
        <LineChartBlock title="Temperature"
                        color="#00BAB6"
                        :icon="temp" />
        <LineChartBlock title="Felt Air"
                        color="#94BF41"
                        :icon="snow" />
        <LineChartBlock title="Humidity"
                        color="#F8B904"
                        :icon="water" />
      </div>
      <div class="row"
           style="margin-top: 40px;">
        <RainfallBlock />
        <SunBlock />
      </div>
    </div>
  </div>
</template>

<script>
import vSelect from 'vue-select';
import 'vue-select/dist/vue-select.css';

import RainfallBlock from './components/RainfallBlock';
import SunBlock from './components/SunBlock';
import LineChartBlock from './components/LineChartBlock';
import snow from './assets/img/snow.svg';
import temp from './assets/img/temp.svg';
import water from './assets/img/water.svg';

export default {
  name: 'app',
  components: {
    RainfallBlock,
    SunBlock,
    LineChartBlock,
    vSelect
  },
  data() {
    return {
      snow,
      temp,
      water,
      selected: ''
    };
  }
};
</script>

<style>
@import './assets/css/normalize.css';
@import './assets/css/index.css';

#app {
  font-family: 'Lato', sans-serif;
  background: #f4f5f7;
  min-height: 100vh;
}

.main-container {
  width: 900px;
  margin: 0 auto;
  padding-top: 40px;
}

.row {
  display: flex;
  justify-content: space-between;
}

.vs__dropdown-toggle {
  background: #ffffff;
}

.vs__open-indicator {
  fill: #979797;
  opacity: 0.3;
}

.vs__selected {
  color: #7e7e7e;
}

.vs__dropdown-option {
  color: #7e7e7e;
}

.vs__dropdown-option--highlight {
  background: rgba(0, 186, 183, 0.7);
  color: white;
}
</style>
