<template>
  <div>
    <div class="container">
      <div class="weather">
        <h2 v-if="localeDate">{{ localeDate }}</h2>
        <h2 v-if="localTime">{{localTime}}</h2>
        <h2 v-if="localTime >= '16:00:00'">Вечер</h2>
        <h2 v-if="localTime <= '16:00:00'">День</h2>
        <h2>Информация о погоде на сегодня</h2>
        <h3>{{ weather.name }}</h3>
        <div class="weather__details" v-if="weather">
          <p v-if="weather && weather.main">Текущая температура: {{ weather.main.temp }}</p>
          <p v-if="weather && weather.speed">Скорость ветра: {{ weather.wind.speed }} м/c</p>
          <img
              v-if="weather && weather.weather"
              :src="`http://openweathermap.org/img/wn/${weather.weather[0].icon}.png`"
          />
          <p v-if="weather && weather.weather">{{ weather.weather[0].description }}</p>

          <p v-if="weather && weather.clouds">Облачность: {{ weather.clouds.all }}%</p>
          <p>Видимость: {{ weather.visibility }}м</p>
          <p v-if="weather && weather.main">Влажность: {{weather.main.humidity}}%</p>
          <p v-if="weather && weather.main">Давление: {{weather.main.pressure}} мм. рт. ст.</p>
        </div>
      </div>
      <div class="nasa">
        <h2>Интересный факт</h2>
        <div class="nasa__details" >
          <img :src="`${infoNasa.url}`" />
          <h3>{{infoNasa.title}}</h3>
          <p>{{infoNasa.explanation}}</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "Home",
  data() {
    return {
      date: "",
      time: '',
      weather: [],
      infoNasa: [],
      coords: {},
    };
  },
  created() {
    this.intervalId = setInterval(() => (this.date = Date.now()), 1000);
    this.intervalTime = setInterval(() => (this.time = Date.now()), 1000);
    this.getCoords();
    this.getNasaInfo()

  },
  beforeUnmount() {
    if (this.intervalId) clearInterval(this.intervalId);
    if (this.intervalTime) clearInterval(this.intervalTime);
  },
  computed: {
    localeDate() {
      return new Date(this.date).toLocaleDateString();
    },
    localTime(){
      return new Date(this.time).toLocaleTimeString();
    }
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

    async getNasaInfo(){
      const { data } = await axios.get('https://api.nasa.gov/planetary/apod?api_key=UCfsuMmrmCawSOsghFcWigWxZS1tjOaAatejVW38')
      if (data){
          this.infoNasa = data
      }
    },

    async getWeather(lat, lon) {
      console.log("Что тут", lat, lon);
      const { data } = await axios.get(
        `https://api.openweathermap.org/data/2.5/weather?lat=${lat}&lon=${lon}&units=metric&lang=ru&appid=d3aa6d2ed9189a443a8d2e823f3fce28`
      );
      if (data) {
        this.weather = data;
      }
    },
  },
};
</script>

<style lang="scss" scoped>
.container{
  width: 1040px;
  margin: 0 auto;
}
.weather{

  h2{
    color: #fff;
  }
 &__details{
   p{
     color: #fff;
   }
 }
}
.nasa{
  h2{
    color: #fff;
  }
  &__details{
    img{
      width: 300px;
    }
    h3{
      color: #fff;
    }
    p{
      color: #fff;
    }
  }
}

</style>