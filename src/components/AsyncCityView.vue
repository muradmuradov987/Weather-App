<template>
  <div
    v-if="Object.keys(weatherData).length > 0"
    class="info__container position-relative d-flex flex-column justify-content-between z-index10"
  >
    <!--Weather Overview-->
    <div class="container">
      <div class="row mt-3 mt-md-5">
        <div class="col-md-6">
          <div class="time__info text-center">
            <h1>
              {{ this.cityTime }} <span>{{ this.amOrPm }}</span>
            </h1>
            <h4>{{ this.cityDate }}</h4>
          </div>
        </div>
        <div class="col-md-6">
          <div
            class="country__info d-flex justify-content-center justify-content-md-end"
          >
            <div>
              <h1>{{ this.$route.params.city }}</h1>
              <h4>{{ this.$route.params.state }}</h4>
            </div>
          </div>
        </div>
      </div>
    </div>
    <div class="weather__info">
      <div class="container">
        <div class="row">
          <div class="col-xl-4 mb-4 mb-xl-0">
            <div class="d-flex ">
              <div class="weather__img">
                <img :src="iconLink" alt="" />
              </div>
              <div>
                <h4 class="text-white">
                  {{ this.weatherData.weather[0].main }}
                </h4>
                <h1 class="text-white">{{ this.temp }}<span>&#176;</span></h1>
                <p class="text-white">
                  {{ this.weatherData.weather[0].description }}
                </p>
              </div>
            </div>
          </div>
          <div class="col-xl-4 mb-4 mb-xl-0">
            <div>
              <div class="d-flex justify-content-between mb-3">
                <div class="w-50 d-flex">
                  <i
                    class="text-white me-3 fs-5 fa-solid fa-temperature-half"
                  ></i>
                  <div>
                    <p class="text-white fs-5 m-0">Real Feel</p>
                    <h2 class="text-white fs-2">
                      {{ this.realFeel }}<span>&#176;</span>
                    </h2>
                  </div>
                </div>
                <div class="w-50 d-flex">
                  <i class="text-white me-3 fs-5 fa-solid fa-wind"></i>
                  <div>
                    <p class="text-white fs-5 m-0">Wind</p>
                    <h2 class="text-white fs-2">{{ this.wind }} km/h</h2>
                  </div>
                </div>
              </div>
              <div class="d-flex justify-content-between">
                <div class="w-50 d-flex">
                  <i class="text-white me-3 fs-5 fa-solid fa-droplet"></i>
                  <div>
                    <p class="text-white fs-5 m-0">Humidity</p>
                    <h2 class="text-white fs-2">{{ this.humidity }} %</h2>
                  </div>
                </div>
                <div class="w-50 d-flex">
                  <i
                    class="text-white me-3 fs-5 fa-solid fa-arrows-down-to-line"
                  ></i>
                  <div>
                    <p class="text-white fs-5 m-0">Pressure</p>
                    <h2 class="text-white fs-2">{{ this.pressure }} mb</h2>
                  </div>
                </div>
              </div>
            </div>
          </div>
          <div class="col-xl-4">
            <div
              class="d-flex align-items-center justify-content-between h-100"
            >
              <div class="sunrise">
                <i class="fs-5 text-white fa-solid fa-sun"></i>
                <p class="text-white">Sunrise</p>
                <span class="text-white">{{ this.sunrise }}</span>
              </div>
              <div class="sunmoon">
                <div
                  class="sun-path"
                  :style="{ transform: 'rotate(' + sunPosition + 'deg)' }"
                >
                  <span class="symbol">☀</span>
                </div>
              </div>
              <div class="sunset">
                <i class="fs-5 text-white fa-solid fa-moon"></i>
                <p class="text-white">Sunset</p>
                <span class="text-white">{{ this.sunset }}</span>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
    <img class="bg" :src="imageSrc" alt="" />
  </div>
</template>

<script>
import axios from "axios";
import moment from "moment";

export default {
  data() {
    return {
      weatherData: {},
      cityTime: null,
      amOrPm: null,
      cityDate: null,
      iconLink: "",
      temp: null,
      realFeel: null,
      humidity: null,
      wind: null,
      pressure: null,
      sunrise: null,
      sunset: null,
      weatherType: "",
      imageName: "",
    };
  },
  methods: {
    async getWeatherData() {
      await axios
        .get(
          `https://api.openweathermap.org/data/2.5/weather?lat=${this.$route.query.lat}&lon=${this.$route.query.lng}&appid=3f7d5c61bad4b8be5a7f10c93e76fb42&units=metric`
        )
        .then((res) => {
          this.weatherData = res.data;
          this.getDateTime();
          this.getIcon();
          this.temp = Math.round(this.weatherData.main.temp);
          this.realFeel = Math.round(this.weatherData.main.feels_like);
          this.humidity = Math.round(this.weatherData.main.humidity);
          this.pressure = Math.round(this.weatherData.main.pressure);
          this.wind = Math.round(this.weatherData.wind.speed);
          this.getSunriseSunset();
          this.setBackground();
        })
        .catch((err) => {
          console.log(err);
        });
    },

    getDateTime() {
      // CALC CURRENT TIME
      const localOffset = new Date().getTimezoneOffset() * 60000;
      const utc = this.weatherData.dt * 1000 + localOffset;
      this.weatherData.currentTime = utc + 1000 * this.weatherData.timezone;
      const currentTime = new Date(this.weatherData.currentTime);
      let hours = currentTime.getHours();
      const minutes = currentTime.getMinutes();
      const amOrPm = hours >= 12 ? "PM" : "AM";
      const formattedTime24h = `${hours.toString().padStart(2, "0")}:${minutes
        .toString()
        .padStart(2, "0")}`;
      this.cityTime = formattedTime24h;
      this.amOrPm = amOrPm;

      // CALC CURRENT DATE
      const currentDate = new Date(
        (this.weatherData.dt + this.weatherData.timezone) * 1000
      );
      const daysOfWeek = [
        "Sunday",
        "Monday",
        "Tuesday",
        "Wednesday",
        "Thursday",
        "Friday",
        "Saturday",
      ];
      const monthsOfYear = [
        "January",
        "February",
        "March",
        "April",
        "May",
        "June",
        "July",
        "August",
        "September",
        "October",
        "November",
        "December",
      ];
      const dayOfWeek = daysOfWeek[currentDate.getDay()];
      const dayOfMonth = currentDate.getDate();
      const monthOfYear = monthsOfYear[currentDate.getMonth()];
      const year = currentDate.getFullYear();
      this.cityDate = `${dayOfWeek}, ${dayOfMonth} ${monthOfYear} ${year}`;
    },
    getIcon() {
      let iconId = this.weatherData.weather[0].icon;
      this.iconLink = `https://openweathermap.org/img/wn/${iconId}@2x.png`;
    },

    getSunriseSunset() {
      let timezone = this.weatherData.timezone;
      let sunrise = this.weatherData.sys.sunrise;
      let sunset = this.weatherData.sys.sunset;
      this.sunrise = moment
        .utc(sunrise, "X")
        .add(timezone, "seconds")
        .format("HH:mm");
      this.sunset = moment
        .utc(sunset, "X")
        .add(timezone, "seconds")
        .format("HH:mm");
    },
    setBackground() {
      //Calc current time minutes
      const time = this.cityTime;
      const [hours, minutes] = time.split(":");
      const totalTime = parseInt(hours, 10) * 60 + parseInt(minutes, 10);

      //Calc sunrise time minutes
      const sunriseTime = this.sunrise;
      const [sunriseHours, sunriseMinutes] = sunriseTime.split(":");
      const totalSunrise =
        parseInt(sunriseHours, 10) * 60 + parseInt(sunriseMinutes, 10);

      //Calc sunrise time minutes
      const sunsetTime = this.sunset;
      const [sunsetHours, sunsetMinutes] = sunsetTime.split(":");
      const totalSunset =
        parseInt(sunsetHours, 10) * 60 + parseInt(sunsetMinutes, 10);

      //Detect day type
      if (totalTime < totalSunrise || totalTime >= totalSunset) {
        this.weatherType = "night";
      } else {
        this.weatherType = "day";
      }

      //Detect weather status
      if (
        this.weatherData.weather[0].main == "Sunny" ||
        this.weatherData.weather[0].main == "Clear"
      ) {
        this.imageName = "sunny.jpg";
      } else if (this.weatherData.weather[0].main == "Clouds") {
        this.imageName = "clouds.jpg";
      } else if (this.weatherData.weather[0].main == "Snow") {
        this.imageName = "snow.jpg";
      } else if (this.weatherData.weather[0].main == "Mist") {
        this.imageName = "mist.jpg";
      } else if (this.weatherData.weather[0].main == "Rain") {
        this.imageName = "rain.jpg";
      } else if (this.weatherData.weather[0].main == "Thunderstorm") {
        this.imageName = "thunderstorm.jpg";
      } else {
        this.imageName = "bg.jpg";
      }
    },
  },
  computed: {
    imageSrc() {
      return require(`../assets/weather/${this.weatherType}/${this.imageName}`);
    },
    sunPosition() {
      const now = new Date(this.weatherData.currentTime);
      const sunriseTime = new Date(`${now.toDateString()} ${this.sunrise}`);
      const sunsetTime = new Date(`${now.toDateString()} ${this.sunset}`);
      const totalTime = sunsetTime - sunriseTime;
      const elapsedTime = now - sunriseTime;
      const degree = (elapsedTime / totalTime) * 180;
      return degree;
    },
  },
  mounted() {
    this.getWeatherData();
  },
};
</script>

<style lang="css" scoped>
.info__container {
  height: calc(100vh - 50px);
  z-index: 10;
}

.bg {
  width: 100%;
  height: 100%;
  object-fit: cover;
  position: absolute;
  top: 0;
  left: 0;
  z-index: -1;
}

.time__info h1 span {
  font-size: 40px;
}

.time__info h1,
.country__info h1 {
  color: white;
  font-size: 70px;
  z-index: 10 !important;
}

.time__info h4,
.country__info h4 {
  color: white;
  font-size: 30px;
}

.weather__info {
  padding: 30px 0px;
  background: rgba(48, 48, 58, 0.5);
  backdrop-filter: blur(2.5px);
}

.weather__img {
  width: 100px;
  height: 100px;
}

.weather__img img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.sunmoon {
  width: 200px;
  height: 100px;
  margin: 0 20px;
  border-bottom: 2px solid white;
  overflow: hidden;
  padding: 8px;
}

.sun-path {
  width: 180px;
  height: 180px;
  border-radius: 50%;
  position: relative;
  border: 2px solid white;
}

.symbol {
  position: absolute;
  left: -12px;
  top: 50%;
  transform: translate(0%, -50%);
  z-index: 10;
}

@media (max-width: 768px) {
  .time__info {
    margin-bottom: 30px;
  }
  .time__info h1 {
    font-size: 30px;
  }
  .time__info h1 span {
    font-size: 20px;
  }

  .time__info h1,
  .country__info h1 {
    font-size: 30px;
  }

  .time__info h4,
  .country__info h4 {
    font-size: 20px;
  }
}
</style>
