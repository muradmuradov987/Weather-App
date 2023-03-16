<template>
  <div
    class="info__container position-relative d-flex flex-column justify-content-between z-index10"
  >
    <img class="bg" src="../assets/bg.jpg" alt="" />

    <!--Banner-->
    <p class="info_msj">
      You are previewing this city, click this '+' icon to start tracking this
      city.
    </p>

    <!--Weather Overview-->

    <div class="container">
      <div class="row">
        <div class="col-md-6">
          <div class="time__info">
            <h1>12:30 <span>PM</span></h1>
            <h4>Monday, 2 February 2023</h4>
          </div>
        </div>
        <div class="col-md-6">
          <div class="country__info d-flex justify-content-end">
            <div>
              <h1>{{ this.$route.params.city }}</h1>
              <h4>{{ this.$route.params.state }}</h4>
            </div>
          </div>
        </div>
      </div>
    </div>
    <div class="container">
      <div class="row">
        <div class="col-md-6">
          <p class="city__info">
            Lorem ipsum dolor sit amet consectetur adipisicing elit. Deleniti
            nobis, ducimus omnis adipisci, modi aliquam quasi in ullam
            temporibus dolorem eos corporis, eligendi libero obcaecati.
            Repellendus aliquid harum autem non.
          </p>
        </div>
      </div>
    </div>
    <div class="weather__info">
      <div class="container">
        <div class="row">
          <div class="col-md-4">
            <div class="d-flex">
              <div class="weather__img">
                <img src="../assets/1.png" alt="" />
              </div>
              <div>
                <h4 class="text-white">Rainy</h4>
                <h1 class="text-white">12 <span>&#176;</span></h1>
              </div>
            </div>
          </div>
          <div class="col-md-4">
            <div >
              <div class=" d-flex justify-content-between mb-3">
                <div class="w-50 d-flex">
                  <i
                    class="text-white me-3 fs-5 fa-solid fa-temperature-half"
                  ></i>
                  <div>
                    <p class="text-white fs-5 m-0">Real Feel</p>
                    <h2 class="text-white fs-2">30 <span>&#176;</span></h2>
                  </div>
                </div>
                <div class="w-50 d-flex">
                  <i class="text-white me-3 fs-5 fa-solid fa-wind"></i>
                  <div>
                    <p class="text-white fs-5 m-0">Wind</p>
                    <h2 class="text-white fs-2">0.2 km/h</h2>
                  </div>
                </div>
              </div>
              <div class=" d-flex justify-content-between">
                <div class="w-50 d-flex">
                  <i class="text-white me-3 fs-5 fa-solid fa-droplet"></i>
                  <div>
                    <p class="text-white fs-5 m-0">Chance of rain</p>
                    <h2 class="text-white fs-2">0%</h2>
                  </div>
                </div>
                <div class="w-50 d-flex">
                  <i class="text-white me-3 fs-5 fa-solid fa-sun"></i>
                  <div>
                    <p class="text-white fs-5 m-0">UV index</p>
                    <h2 class="text-white fs-2">3</h2>
                  </div>
                </div>
              </div>
            </div>
          </div>
          <div class="col-md-4">
            <h1 class="text-white">shvfshf</h1>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      weatherData: null,
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
          console.log(this.weatherData);
        })
        .catch((err) => {
          console.log(err);
        });
    },
  },
  mounted() {
    this.getWeatherData();
    // this.calcTime();
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
.info_msj {
  text-align: center;
  padding: 15px 0px;
  background: #004e71;
  color: white;
  z-index: 10 !important;
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

.city__info {
  color: white;
  font-size: 20px;
}
.weather__info {
  padding: 30px 0px;
  background: rgba(24, 24, 133, 0.3);
  backdrop-filter: blur(5.5px);
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

</style>
