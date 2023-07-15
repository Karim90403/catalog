<template>
  <h1>Категории товаров</h1>
  <h2>{{ cityName }}</h2>
  <input type="text" v-model="searchInput">
  <button @click=""></button>
  <p v-for="city in cityArr" :key="city.id" @click="changeCity(city.id, city.city)">{{ city.label }}</p>
  <div class="category-wrapper">
    <div class="category-container">
      <div v-for="tag in menuTags" :key="tag.id" @click="loadCtegory(tag.slug, tag.name)" class="category-item">
        <img :src="`${tag.image}`" :alt="`${tag.name}`">
        <span :style="'color:' + tag.text_color + ';'" class="category-item_name">{{tag.name}}</span>
      </div>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import axios from 'axios';
export default {
  name: 'HomeView',
  components: {},
  data(){
    return {
      menuTags: [],
      cityArr: [],
      cityName: '',
      searchInput: ''
    }
  },
  methods: {
    async loadMenu(city_id){
      try {
        let res = await axios.get(`https://nlstar.com/ru/api/catalog3/v1/menutags/?city_id=${city_id}`)
        this.menuTags = res.data.tags;
        this.cityName = localStorage.getItem("city") ?? "Новосибирск";
      } catch (error) {
        alert(error);
      }
    },
    loadCtegory(slug, name){
      this.$router.push({
        name: "category",
        query: {
          slug: slug,
          name: name
        }
      })
    },
    async getCity(term){
      try {
        let res = await axios.get(`https://nlstar.com/api/catalog3/v1/city/?country=ru&term=${term}`)
        this.cityArr = res.data.data;
      } catch (error) {
        alert(error);
      }
    },
    changeCity(id, city) {
      localStorage.setItem("city", city);
      this.loadMenu(id);
    }
  },
  async mounted(){
    this.loadMenu(1);
    this.getCity("Мос")
  }
}
</script>

<style scoped>
@-webkit-keyframes flash {
	0% {
		opacity: .4;
	}
	100% {
		opacity: 1;
	}
}
@keyframes flash {
	0% {
		opacity: .4;
	}
	100% {
		opacity: 1;
	}
}
.category-wrapper{
  max-width: 100vw;
  display: flex;
  justify-content: center;
}
.category-container{
  width: 60vw;
  display: grid;
  grid-template-columns: repeat(4,1fr);
  grid-gap: 12px;
}
.category-item{
  position: relative;
  max-width: fit-content;
  cursor: pointer;
}

.category-item:hover img{
  opacity: 1;
	-webkit-animation: flash 1.5s;
	animation: flash 1.5s;
}

.category-item img{
  width: 100%;
}

.category-item_name{
  position: absolute;
  width: 70%;
  top: 20px;
  left: 20px;
}
</style>
