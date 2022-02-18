<template>
  <div class="nasa">
    <UiCard class="nasa__card">
      <div v-if="isLoading">Загружаю информацию...</div>
      <div v-if="!nasa">Не смогли загрузить инфиормацию :(</div>
    <div v-if="!isLoading && nasa">
      <h2>Интересный факт</h2>
      <div class="nasa__title">
        <img :src="`${nasa.url}`" :alt="`${nasa.title}`"/>
        <h3>{{ nasa.title }}</h3>
      </div>
      <div class="nasa__details">
        <p>{{ nasa.explanation }}</p>
      </div>
    </div>
    </UiCard>
  </div>
</template>

<script>
import UiCard from "@/components/UI/card/uiCard";
export default {
  name: "nasa",
  components: { UiCard },
  props: {
    nasa: {
      type: Object,
      require: true
    },
    isLoading: {
      type: Boolean
    }
  },
};
</script>

<style lang="scss" scoped>
.nasa {
  h2 {
    font-family: Sarabun, sans-serif;
    font-style: normal;
    font-weight: 500;
    font-size: 25px;
    line-height: 32px;
    text-align: center;

    color: #ffffff;
    margin: 10px;
  }
  &__title {
    display: flex;
    justify-content: space-evenly;
    align-items: center;
    img {
      width: 400px;
      height: 280px;
      border-radius: 30px;
      margin: 10px;
    }
  }
  &__card {
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
      margin: 10px;
    }
    p {
      font-family: Sarabun, sans-serif;
      font-style: normal;
      font-weight: 500;
      font-size: 16px;
      line-height: 32px;
      text-align: center;

      color: #ffffff;
      margin: 10px;
    }
  }
}
</style>
