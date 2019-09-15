<template>
  <div id="app">
    <div class="main-container">
      <v-select v-model="selected"
                :options="cityData"
                :clearable="false"
                :searchable="false"
                style="width: 201px;"></v-select>
      <div style="margin-top: 28px;"
           v-if="!loading">
        <div class="row">
          <LineChartBlock title="Temperature"
                          color="#00BAB6"
                          :icon="temp"
                          :chartData="getTempChartData"
                          :avg="getAvgTemp" />
          <LineChartBlock title="Felt Air"
                          color="#94BF41"
                          :icon="snow"
                          :chartData="getFeltAirChartData"
                          :avg="getAvgFeltAir" />
          <LineChartBlock title="Humidity"
                          color="#F8B904"
                          :icon="water"
                          :chartData="getHumidityChartData"
                          :avg="getAvgHumidity" />
        </div>
        <div class="row"
             style="margin-top: 40px;">
          <RainfallBlock :chartData="getRainfallData" />
          <SunBlock :sunset="getSun.sunset" :sunrise="getSun.sunrise"/>
        </div>
      </div>
      <div class="loading"
           v-else>
        <div class="lds-ripple">
          <div></div>
          <div></div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import vSelect from 'vue-select';
import moment from 'moment';
import sum from 'lodash/sum';
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
      selected: '',
      cityData: [],
      weatherData: {},
      loading: true
    };
  },
  computed: {
    getRainfallData() {
      const { histories } = this.weatherData;
      if (!histories) return [];
      return histories.map(item => ({
        name: item.at.substring(11, 16),
        value: item.rainfall
      }));
    },
    getTempChartData() {
      const { histories } = this.weatherData;
      return histories.map(item => ({
        name: moment(item.at).format('mm:ss'),
        value: item.temperature
      }));
    },
    getFeltAirChartData() {
      const { histories } = this.weatherData;
      return histories.map(item => ({
        name: moment(item.at).format('mm:ss'),
        value: item.felt_air_temp
      }));
    },
    getHumidityChartData() {
      const { histories } = this.weatherData;
      return histories.map(item => ({
        name: moment(item.at).format('mm:ss'),
        value: item.humidity
      }));
    },
    getAvgTemp() {
      const { histories } = this.weatherData;
      return (sum(histories.map(item => item.temperature)) / histories.length).toFixed(1);
    },
    getAvgFeltAir() {
      const { histories } = this.weatherData;
      return (sum(histories.map(item => item.felt_air_temp)) / histories.length).toFixed(1);
    },
    getAvgHumidity() {
      const { histories } = this.weatherData;
      return (sum(histories.map(item => item.humidity)) / histories.length).toFixed(1);
    },
    getSun() {
      return {
        sunset: this.weatherData.sunset,
        sunrise: this.weatherData.sunrise
      }
    }
  },
  created() {
    this.getCityData();
  },
  watch: {
    selected() {
      this.getWeatherData(this.selected.value);
    }
  },
  methods: {
    getCityData() {
      axios
        .get('https://works.ioa.tw/weather/api/all.json')
        .then(response => {
          this.cityData = response.data.map(city => ({ label: `${city.name}, 台灣`, value: city.id }));
          this.selected = this.cityData[0];
        })
        .catch(error => {
          // eslint-disable-next-line
          console.error(error);
        });
    },
    getWeatherData(id) {
      this.loading = true;
      axios
        .get(`https://works.ioa.tw/weather/api/weathers/${id}.json`)
        .then(response => {
          this.loading = false;
          this.weatherData = response.data;
        })
        .catch(error => {
          this.loading = false;
          // eslint-disable-next-line
          console.error(error);
        });
    }
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
  padding: 10px;
  color: #7e7e7e;
}

.vs__dropdown-option--highlight {
  background: rgba(0, 186, 183, 0.7);
  color: white;
}

.loading {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}

.lds-ripple {
  display: inline-block;
  position: relative;
  width: 64px;
  height: 64px;
}
.lds-ripple div {
  position: absolute;
  border: 4px solid #00bab6;
  opacity: 1;
  border-radius: 50%;
  animation: lds-ripple 1s cubic-bezier(0, 0.2, 0.8, 1) infinite;
}
.lds-ripple div:nth-child(2) {
  animation-delay: -0.5s;
}
@keyframes lds-ripple {
  0% {
    top: 28px;
    left: 28px;
    width: 0;
    height: 0;
    opacity: 1;
  }
  100% {
    top: -1px;
    left: -1px;
    width: 58px;
    height: 58px;
    opacity: 0;
  }
}
</style>
