<template>
  <h2 class="city" @click="showPopup = true">{{ cityName }}</h2>
  <router-view/>
  <div v-show="showPopup" class="city-popup_container">
    <div @click="showPopup = false" class="city-popup_mask"></div>
    <div class="city-popup">
      <input class="city-popup_input" type="text" v-model="searchInput" @input="getCity()">
      <button class="city-popup_button" @click="changeCity()" :disabled="!buttonActive">Подтверить</button>
      <div class="city-popup_list">
        <span v-for="city in cityArr" :key="city.id" @click="chooseCity(city.id, city.city, city.label)">{{ city.label }}</span>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data(){
    return{
      cityArr: [],
      cityName: '',
      searchInput: '',
      id: 0,
      showPopup: false,
    }
  },
  methods:{
    async getCity(){
      if(this.searchInput.length >= 3){
        try {
          let res = await axios.get(`https://nlstar.com/api/catalog3/v1/city/?country=ru&term=${this.searchInput}`)
          this.cityArr = res.data.data;
        } catch (error) {
          alert(error);
        }
      }
    },
    changeCity() {
      this.cityName = localStorage.getItem("city");
      this.loadMenu(this.id);
      this.showPopup = false;
    },
    chooseCity(id, city, label) {
      localStorage.setItem("city", city);
      localStorage.setItem("id", id);
      this.id = id;
      this.searchInput = label;
      this.buttonActive = true;
    }
  },
  mounted(){
    this.cityName = localStorage.getItem("city") ?? "Новосибирск";
  }
}
</script>

<style>
 .city{
  box-shadow: 0px 2px 4px 0px rgba(39, 39, 39, 0.1);
}

.city-popup {
  position: relative;
  overflow: hidden;
  color: #000;
  background-color: #fff;
  box-shadow: 0px 2px 10px 0px rgba(151, 151, 151, 0.2); 
  width: 769px;
  height: 144px;
  border-radius: 4px;
}

.city-popup_mask {
  position: fixed;
  display: flex;
  cursor: pointer;
  background-color: rgba(53, 54, 71, 0.5);
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
}

.city-popup_container{
  position: fixed;
  display: flex;
  justify-content: center;
  align-items: center;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
}

.city-popup_list {
  display: flex;
  flex-direction: column;
}
</style>
