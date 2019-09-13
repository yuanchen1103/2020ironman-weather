<template>
  <div class="wrapper">
    <h1 class="title">
      Sunrise and Sunset
    </h1>
    <div class="row">
      <div class="group">
        <img :src="sunriseImg"
             alt="">
        <h5 class="data">06:32</h5>
      </div>
      <div class="group">
        <img :src="sunsetImg"
             alt="">
        <h5 class="data">17:08</h5>
      </div>
    </div>
  </div>
</template>

<script>
import sunsetImg from '../assets/img/sunset.svg';
import sunriseImg from '../assets/img/sunrise.svg';

export default {
  name: 'SunBlock',
  data() {
    return {
      sunsetImg,
      sunriseImg
    };
  }
};
</script>

<style scoped>
.wrapper {
  background: #ffffff;
  border-radius: 8px;
  padding: 20px 32px 15px 32px;
  width: 260px;
}
.title {
  font-size: 16px;
  color: #5e565a;
  font-weight: 400;
  margin: 0;
}
.row {
  display: flex;
  justify-content: space-between;
  width: 210px;
  margin: 47px auto 0 auto;
}
.group {
  display: flex;
  flex-direction: column;
  align-items: center;
}
.data {
  font-size: 30px;
  color: #a0a0a0;
  font-weight: 400;
  margin-top: 19px;
  margin-bottom: 0;
}
</style>