<template>
  <span class="title">Категории товаров</span>
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
      buttonActive: false
    }
  },
  methods: {
    async loadMenu(city_id){
      try {
        let res = await axios.get(`https://nlstar.com/ru/api/catalog3/v1/menutags/?city_id=${city_id}`)
        this.menuTags = res.data.tags;
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
  },
  async mounted(){
    if (localStorage.getItem("id")) {
      this.loadMenu(localStorage.getItem("id"));
    }
    else {
      this.loadMenu(1);
    }
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

@font-face {
  font-family: "FuturaPT";
  src: local("FuturaPT"),
    url(../fonts/FuturaPT.ttf) format("truetype");
}

.title{
  font-family: "FuturaPT";
  font-size: 44px;
  margin-left: 20vw;
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
  max-height: fit-content;
  cursor: pointer;
  overflow: hidden;
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
  font-family: "FuturaPT";
}
</style>
