<template>
  <div>
    <div class="search-box">
      <UiInput type="text" class="search-box__input" placeholder="Что хотите посмотреть ?"  v-model="searchQuery"/>
      <ui-button class="search-box__button" @click="runSearch">Найти</ui-button>
    </div>
    <div v-if="foundComplete" class="results">
      <div class="results__item" v-for="item in results.results" :key="item.id">
        <img style="width: 100px" :src="`${item.cover_photo.urls.full}`" />
        <h2>{{item.title}}</h2>
        <p>Имя автора: {{item.user.first_name}}</p>
      </div>
    </div>
    <div v-if="notFound" class="results-error"><h2>Ничего не найдено</h2></div>

  </div>
</template>
<script>
import axios from "axios";
import UiInput from "@/components/UI/input/uiInput";
import UiButton from "@/components/UI/button/uiButton";

export default {
  name: "photos",
  components: {UiButton, UiInput},
  data() {
    return {
      searchQuery: "",
      results: [],
    };
  },
  computed:{
    foundComplete(){
      return this.results && this.results.results && this.results.results.length
    },
    notFound(){
      return this.results && this.results.results && !this.results.results.length
    }
  },
  methods: {
    runSearch(){
      this.searchPhotos(this.searchQuery)
    },


    async searchPhotos(searchQuery) {

      const { data } = await axios.get(
        `https://api.unsplash.com/search/collections?page=1&query=${searchQuery}`,
        {
          headers: {
            Authorization:
              "Client-ID M6IhqP2ZPQF7UafDeaHohGqblMIlZ4u7zqGQfmspNIA",
          },
        }
      );
      if (data){
        console.log(data)
        this.results = data;
      }
    },
  },
};
</script>

<style lang="scss" scoped>
.search-box{
  display: flex;
  justify-content: center;

  &__input{
    margin: 0;
    border-radius: 10px 0 0 10px;
    width: 686px;
    color: #fff;
  }
  &__button{
    margin: 0;
    border-radius: 0 10px 10px 0;
  }
}
.results-error{
  margin: 20px auto;
  color: #fff;

}
.results{
  display: grid;
  grid-template-columns:  repeat(4, 1fr);
  &__item{
    margin: 20px 10px;
  }

  p{
    color: #FFF;
  }
 h2{
   color: #fff;
 }
}
</style>
