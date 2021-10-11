<template>
  <div class="weatherCard">
    <img :src="'http://openweathermap.org/img/wn/' + icon + '@2x.png'" />
    <p class="txt">{{ dateTime }}</p>
    <p class="txt">{{ temp }}°C</p>
    <p class="txt">{{ description }}</p>
    <p class="txt">{{ speed }} km/h</p>
  </div>
</template>
<script>
export default {
  props: {
    icon: String,
    dateTime: String,
    temp: Number,
    description: String,
    speed: Number,
  },
};
</script>
<style></style>
