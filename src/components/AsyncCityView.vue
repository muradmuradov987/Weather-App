<template>
  <div>
    <button @click="getWeatherData()">Test</button>
   
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      mm: null,
    };
  },
  methods: {
    getWeatherData() {
      axios
        .get(
          `https://api.openweathermap.org/data/2.5/weather?lat=${this.$route.query.lat}&lon=${this.$route.query.lng}&appid=3f7d5c61bad4b8be5a7f10c93e76fb42`
        )
        .then((res) => {
          console.log(res);
        })
        .catch((err) => {
          console.log(err);
        });
    },
  },
  mounted() {
    // this.getWeatherData();
  },
};
</script>

<style lang="css" scoped></style>
