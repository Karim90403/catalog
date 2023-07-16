<template>
  <router-link to="/">{{ $route.query.name }}</router-link>
  <div class="category">
    <div v-if="subcategories.length >= 2" class="subcategory-menu">
      <span class="subcategory-menu_item">Все продукты</span>
      <span v-for="subcategory in subcategories" :key="subcategory.id" @click="loadSubcategoryMenu(subcategory.slug)" class="subcategory-menu_item">{{ subcategory.name }}</span>
    </div>
    <div class="subcategory-container">
      <div v-for="product in products" :key="product.id" class="product">
        <img :src="`${product.image}`" :alt="`${product.full_name}`">
        <span class="product-category"> {{ product.category.name }} </span>
        <span class="product-name">{{ product.present_name }}</span>
        <span class="product-comment"> {{ product.comment_name }} </span>
        <span class="product-price"> {{ product.price }} </span>
        <a v-if="product.allowed && product.available" :href="`https://nlstar.com${product.url}`" target="_blank" class="product-cart_url">В корзину</a>
        <span v-else class="product-cart_inactive">Нет в наличии</span>
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
.category{
  max-width: 100vw;
  display: flex;
  justify-content: center;
}
.subcategory-container{
  width: 60vw;
  display: grid;
  grid-template-columns: repeat(3,1fr);
  grid-gap: 12px;
}

.product{
  position: relative;
  max-width: fit-content;
  display: flex;
  flex-direction: column;
  cursor: pointer;
}

.product img{
  width: 100%;
}

.subcategory-menu{
  display: flex;
  flex-direction: column;
}

.product-category{

}

.product-name{

}

.product-comment{

}

.product-price{

}

.product-cart_url{
 cursor: pointer;
}
</style>