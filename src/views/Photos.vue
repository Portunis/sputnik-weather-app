<template>
  <div>
    <div class="search-box">
      <UiInput
        type="text"
        class="search-box__input"
        placeholder="Что хотите посмотреть ?"
        v-model="searchQuery"
      />
      <ui-button class="search-box__button" @click="runSearch">Найти</ui-button>
    </div>
    <div v-if="!isLoading" class="results">
      <UiCard
        class="results__card"
        v-for="item in results.results"
        :key="item.id"
        :style="{
          backgroundImage: 'url(' + `${item.cover_photo.urls.regular}` + ')',
        }"
      >
        <div class="card__info">
          <h2>{{ item.title }}</h2>
          <p>Имя автора: {{ item.user.first_name }}</p>
        </div>
      </UiCard>
    </div>
    <UiCard
      v-if="placeholder"
      class="results__card"
      :style="{
        backgroundImage: 'url(' + require('@/assets/img/placeholder.png') + ')',
      }"
    >
      <div class="card__info">
        <h2>Я фотография</h2>
        <p>Имя автора: А я твой автор</p>
      </div>
    </UiCard>
    <div v-if="isLoading" class="results-error">
      <h2>Загружаю фотографии...</h2>
    </div>
    <div v-if="notFound" class="results-error">
      <h2>Ничего не найдено :(</h2>
    </div>
  </div>
</template>
<script>
import axios from "axios";
import UiInput from "@/components/UI/input/uiInput";
import UiButton from "@/components/UI/button/uiButton";
import UiCard from "@/components/UI/card/uiCard";

export default {
  name: "photos",
  components: { UiCard, UiButton, UiInput },
  data() {
    return {
      placeholder: true,
      isLoading: false,
      searchQuery: "",
      page: 1,
      results: [],
    };
  },
  computed: {
    notFound() {
      return (
        this.results && this.results.results && !this.results.results.length
      );
    },
  },
  methods: {
    runSearch() {
      this.searchPhotos(this.searchQuery);
    },

    async searchPhotos(searchQuery) {
      try {
        this.placeholder = false;
        this.isLoading = true;
        const { data } = await axios.get(
          `https://api.unsplash.com/search/collections?query=${searchQuery}`,
          {
            params: {
              page: this.page,
            },
            headers: {
              Authorization:
                "Client-ID M6IhqP2ZPQF7UafDeaHohGqblMIlZ4u7zqGQfmspNIA",
            },
          }
        );
        if (data) {
          this.results = data;
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
.search-box {
  display: flex;
  justify-content: center;
  margin: 40px;

  &__input {
    margin: 0;
    border-radius: 10px 0 0 10px;
    width: 686px;
    color: black;
  }
  &__button {
    margin: 0;
    border-radius: 0 10px 10px 0;
  }
}
.results-error {
  margin: 20px auto;
  color: #fff;
}
.card {
  &__info {
    padding: 200px 0;
    text-align: center;
    text-transform: uppercase;
  }
}
.results {
  display: grid;
  justify-items: center;
  grid-template-columns: repeat(4, 1fr);

  &__card {
    padding: 10px;
    background-size: cover;
    background-repeat: no-repeat;
    width: 380px;
    margin: 5px;
  }
  &__item {
    margin: 20px 10px;
  }
  p {
    color: #fff;
  }
  h2 {
    color: #fff;
  }
}
</style>
