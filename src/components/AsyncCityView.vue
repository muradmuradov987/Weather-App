<template>
  <button @click="getWeatherData()">Test</button>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {};
  },
  methods: {
    
    async getWeatherData() {
      try {
        const weatherData = await axios.get(
          `https://api.openweathermap.org/data/2.5/onecall?lat=${this.$route.query.lat}&lon=${this.$route.query.lng}&exclude={part}&appid=bdc97dea47562bd211affdd4e947bb5e&units=imperial`);

          

        // cal current date & time
        const localOffset = new Date().getTimezoneOffset() * 60000;
        const utc = weatherData.data.current.dt * 1000 + localOffset;
        weatherData.data.currentTime =
          utc + 1000 * weatherData.data.timezone_offset;
        // cal hourly weather offset
        weatherData.data.hourly.forEach((hour) => {
          const utc = hour.dt * 1000 + localOffset;
          hour.currentTime = utc + 1000 * weatherData.data.timezone_offset;
        });
        return weatherData;
      } catch (err) {
        console.log(err);
      }
    },
    
    // async weatherData() {
    //   await getWeatherData();
    // },
  },
};
</script>

<style lang="css" scoped></style>
