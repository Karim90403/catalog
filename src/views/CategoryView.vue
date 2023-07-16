<template>
  <router-link class="back" to="/">
    <img src="../assets/back.png">
    {{ $route.query.name }}
  </router-link>
  <div class="subcategory">
    <div class="subcategory-wrapper" :class="{'subcategory-wrapper_grid': subcategories.length >= 2}"> 
      <div v-if="subcategories.length >= 2" class="subcategory-menu">
        <span class="subcategory-menu_item">Все продукты</span>
        <span v-for="subcategory in subcategories" :key="subcategory.id" @click="loadSubcategoryMenu(subcategory.slug)" class="subcategory-menu_item">{{ subcategory.name }}</span>
      </div>
      <div :class="[subcategories.length >= 2 ? 'subcategory-container_small' : 'subcategory-container_big']">
        <div v-for="product in products" :key="product.id" class="product">
          <img :src="`${product.image}`" :alt="`${product.full_name}`">
          <span class="product-category"> {{ product.category.name }} </span>
          <span class="product-name">{{ product.present_name }}</span>
          <span class="product-comment"> {{ product.comment_name }} </span>
          <span class="product-price"> {{ product.price }} ₽</span>
          <a v-if="product.allowed && product.available" :href="`https://nlstar.com${product.url}`" target="_blank" class="product-cart_url">В корзину</a>
          <span v-else class="product-cart_inactive">Нет в наличии</span>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
export default {
  name: 'CategoryView',
  components: {},
  data(){
    return{
      products: [],
      subcategories: [],
    }
  },
  methods:{
    async loadCategoryMenu(){
      try {
        let res = await axios.get(`https://nlstar.com/ru/api/catalog3/v1/menutags/${this.$route.query.slug}/`)
        this.products = res.data.products;
      } catch (error) {
        alert(error);
      }
    },
    async loadSubcategoryMenu(SubcategorySlug){
      try {
        let res = await axios.get(`https://nlstar.com/ru/api/catalog3/v1/menutags/${SubcategorySlug}/`)
        this.products = res.data.products;
      } catch (error) {
        alert(error);
      }
    },
    async loadSubcategories(){
      try {
        let res = await axios.get("https://nlstar.com/ru/api/catalog3/v1/menutags/")
        res.data.tags.forEach(element => {
          if (element.slug == this.$route.query.slug) {
            this.subcategories = element.children;
          }
        });
      } catch (error) {
        alert(error);
      }
    }
  },
  async mounted(){
    this.loadCategoryMenu();
    this.loadSubcategories();
  }
}
</script>

<style scoped>
@font-face {
  font-family: "FuturaPT";
  src: local("FuturaPT"),
    url(../fonts/FuturaPT.ttf) format("truetype");
}
 
@font-face {
  font-family: "FuturaPT regular";
  src: local("FuturaPT regular"),
    url(../fonts/FuturaPT-reg.ttf) format("truetype");
}

.back{
  font-family: "FuturaPT";
  text-decoration: none;
  color: #000;
  font-size: 44px;
  margin-left: 20vw;
}

.back img {
  width: 30px;
  height: 30px;
}

.subcategory{
  max-width: 100vw;
  display: flex;
  justify-content: center;
}

.subcategory-wrapper{
  max-width: 60vw;
}

.subcategory-wrapper_grid{
  display: grid;
  grid-template-columns: 1fr 3fr;
}

.subcategory-container_small{
  display: grid;
  grid-template-columns: repeat(3,1fr);
  grid-gap: 12px;
}

.subcategory-container_big{
  display: grid;
  grid-template-columns: repeat(4,1fr);
  grid-gap: 12px;
}

.product{
  position: relative;
  max-width: fit-content;
  display: flex;
  flex-direction: column;
  cursor: pointer;
  box-shadow: 0px 2px 10px 0px rgba(151, 151, 151, 0.2);
}

.product img{
  width: 100%;
}

.subcategory-menu{
  display: flex;
  flex-direction: column;
  width: 240px;
  height: 321px;
}

.subcategory-menu_item{
  font-family: "FuturaPT regular";
  cursor: pointer;
}
.product-category{
  font-family: "FuturaPT regular";
  font-size: 14px;
  font-weight: 400;
  line-height: 14px;
  letter-spacing: 0px;
  width: 80%;
  margin-left: 10%;
  text-align: center;
  color: rgba(151, 151, 151, 1);
}

.product-name{
  font-family: "FuturaPT";
  font-size: 20px;
  font-weight: 600;
  line-height: 24px;
  letter-spacing: 0px;
  width: 80%;
  margin-left: 10%;
  text-align: center;
  color: rgba(39, 39, 39, 1);
}

.product-comment{
  font-family: "FuturaPT regular";
  margin-top: 10px;
  font-size: 16px;
  font-weight: 400;
  line-height: 20px;
  width: 80%;
  margin-left: 10%;
  letter-spacing: 0px;
  text-align: center;
  color: rgba(39, 39, 39, 1); 
}

.product-price{
  font-family: "FuturaPT";
  margin-top: 25px;
  font-size: 26px;
  font-weight: 600;
  line-height: 24px;
  width: 80%;
  margin-left: 10%;
  letter-spacing: 0px;
  text-align: center;
  color: rgba(39, 39, 39, 1);
}

.product-cart_url{
 cursor: pointer;
 font-family: "FuturaPT";
 background: linear-gradient(270deg, #FFA800 0%, #FF6F00 100%);
 box-shadow: 0px 5px 25px #FFA800;
 text-decoration: none;
 color: #fff;
 padding: 10px 20px;
 text-align: center;
 margin: 4px 2px;
 width: 80%;
 margin-left: 10%;
 box-sizing: border-box;
 margin-top: 15px;
 margin-bottom: 20px;
 border-radius: 30px;
}

.product-cart_inactive {
  font-family: "FuturaPT";
  cursor: not-allowed;
  color: rgba(39, 39, 39, 0.5);
  border: 2px solid rgba(151, 151, 151, 0.3);
  padding: 10px 20px;
  text-align: center;
  margin: 4px 2px;
  width: 80%;
  margin-left: 10%;
  box-sizing: border-box;
  margin-top: 15px;
  margin-bottom: 20px;
  border-radius: 30px;
}
</style>