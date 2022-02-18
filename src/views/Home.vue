<template>
  <div>
    <div class="container">
      <Weather :weather="weather" :isLoading="isLoading" />
      <Nasa :nasa="infoNasa" :isLoading="isLoading" />
    </div>
  </div>
</template>

<script>
import axios from "axios";
import Weather from "@/components/weather";
import Nasa from "@/components/nasa";

export default {
  name: "Home",
  components: { Nasa, Weather },
  data() {
    return {
      isLoading: false,
      weather: [],
      infoNasa: [],
      coords: {},
    };
  },
  created() {
    this.getCoords();
    this.getNasaInfo();
  },
  methods: {
    getCoords() {
      navigator.geolocation.getCurrentPosition(
        (position) => {
          this.getWeather(position.coords.latitude, position.coords.longitude);
        },
        (error) => {
          console.log(error.message);
        }
      );
    },

    async getNasaInfo() {
      try {
        this.isLoading = true;
        const { data } = await axios.get(
          "https://api.nasa.gov/planetary/apod?api_key=UCfsuMmrmCawSOsghFcWigWxZS1tjOaAatejVW38"
        );
        if (data) {
          this.infoNasa = data;
        }
      } catch (e) {
        console.log(e);
      } finally {
        this.isLoading = false;
      }
    },

    async getWeather(lat, lon) {
      try {
        this.isLoading = true;
        const { data } = await axios.get(
          `https://api.openweathermap.org/data/2.5/weather?lat=${lat}&lon=${lon}&units=metric&lang=ru&appid=d3aa6d2ed9189a443a8d2e823f3fce28`
        );
        if (data) {
          this.weather = data;
        }
      } catch (e) {
        console.log(e);
      } finally {
        this.isLoading = false;
      }
    },
  },
};
</script>

<style lang="scss" scoped>
body {
  background-size: cover !important;
  background: rgb(125, 134, 144) !important;
  background: linear-gradient(
    90deg,
    rgba(125, 134, 144, 1) 25%,
    rgba(115, 93, 103, 1) 86%
  ) !important;
  height: 100%;
}

.container {
  width: 1040px;
  margin: 0 auto;
}
</style>
