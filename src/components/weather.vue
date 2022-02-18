<template>
  <div class="weather">
    <UiCard class="weather__card">
      <div class="weather__dateTime">
        <h2 v-if="localTime >= '16:00:00'">Вечер</h2>
        <h2 v-if="localTime <= '16:00:00'">День</h2>
        <h2>Информация о погоде на:</h2>
        <h2 v-if="localeDate">{{ localeDate }}</h2>
        <h2 v-if="localTime">{{ localTime }}</h2>
      </div>
    </UiCard>

    <UiCard class="weather__card">
      <div v-if="isLoading">Загружаю информацию...</div>
      <div v-if="!weather">Не смогли загрузить инфиормацию :(</div>
      <div class="weather__details" v-if="!isLoading && weather">
        <img
          v-if="weather && weather.weather"
          :src="`http://openweathermap.org/img/wn/${weather.weather[0].icon}.png`"
          alt="icon-weather"
        />
        <h3>Город: {{ weather.name }}</h3>
        <div class="details">
          <p v-if="weather && weather.main">
            Текущая температура: {{ weather.main.temp }}
          </p>
          <p v-if="weather && weather.main">
            Ощущается как: {{ weather.main.feels_like }}
          </p>
          <p v-if="weather && weather.speed">
            Скорость ветра: {{ weather.wind.speed }} м/c
          </p>

          <p v-if="weather && weather.weather">
            Осадки: {{ weather.weather[0].description }}
          </p>

          <p v-if="weather && weather.clouds">
            Облачность: {{ weather.clouds.all }}%
          </p>
          <p>Видимость: {{ weather.visibility }}м</p>
          <p v-if="weather && weather.main">
            Влажность: {{ weather.main.humidity }}%
          </p>
          <p v-if="weather && weather.main">
            Давление: {{ weather.main.pressure }} мм. рт. ст.
          </p>
        </div>
      </div>
    </UiCard>
  </div>
</template>

<script>
import UiCard from "@/components/UI/card/uiCard";

export default {
  name: "weather",
  components: { UiCard },
  data() {
    return {
      date: "",
      time: "",
    };
  },
  props: {
    weather: {
      type: Object,
      require: true
    },
    isLoading: {
      type: Boolean
    }
  },
  created() {
    this.intervalId = setInterval(() => (this.date = Date.now()), 1000);
    this.intervalTime = setInterval(() => (this.time = Date.now()), 1000);

  },
  computed: {
    localeDate() {
      return new Date(this.date).toLocaleDateString();
    },
    localTime() {
      return new Date(this.time).toLocaleTimeString();
    },
  },
  beforeUnmount() {
    if (this.intervalId) clearInterval(this.intervalId);
    if (this.intervalTime) clearInterval(this.intervalTime);
  },
};
</script>

<style lang="scss" scoped>
.weather {
  h2 {
    width: 0;
    margin: 20px;
    background: none;

    font-family: Sarabun, sans-serif;
    font-style: normal;
    font-weight: 500;
    font-size: 25px;
    line-height: 32px;
    text-align: center;

    color: #ffffff;
  }
  &__dateTime {
    width: auto;
    background: none;
  }
  &__card {
    margin: 10px auto;
    h2 {
      width: 100%;
      font-family: Sarabun, sans-serif;
      font-style: normal;
      font-weight: 500;
      font-size: 25px;
      line-height: 32px;
      text-align: center;

      color: #ffffff;
    }
    width: 100%;
  }
  &__details {
    h3 {
      font-family: Sarabun, sans-serif;
      font-style: normal;
      font-weight: 500;
      font-size: 25px;
      line-height: 32px;
      text-align: center;

      color: #ffffff;
    }
    p {
      font-family: Sarabun, sans-serif;
      font-style: normal;
      font-weight: 500;
      font-size: 16px;
      line-height: 32px;
      text-align: center;

      color: #ffffff;
    }
  }
}
.details {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  p {
    text-align: left;
    margin: 20px;
  }
}
</style>
