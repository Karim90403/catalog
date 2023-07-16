<template>
  <div class="city" @click="showPopup = true">
    <img src="../src/assets/geoposition.png">
    {{ cityName }}</div>
  <router-view/>
  <div v-show="showPopup" class="city-popup_container">
    <div @click="showPopup = false" class="city-popup_mask"></div>
    <div class="city-popup">
      <span class="city-popup_title">Выбор населённого пункта:</span>
      <p>
        <input type="text" v-model="searchInput" @input="getCity()" placeholder="Например, Санкт-петербург" class="city-popup_input">
        <span @click="changeCity()" :class="buttonActive ? 'city-popup_button_active' : 'city-popup_button_disabled'">Подтверить</span>
      </p>
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
      if(this.searchInput.length >= 3){
        this.cityName = localStorage.getItem("city");
        this.loadMenu(this.id);
        this.showPopup = false;
      }
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

<style scoped>
@font-face {
  font-family: "FuturaPT";
  src: local("FuturaPT"),
    url(./fonts/FuturaPT.ttf) format("truetype");
}

 .city {
  font-family: "FuturaPT";
  display: flex;
  align-items: center;
  height: 7vh;
  width: 100vw;
  box-sizing: border-box;
  padding-left: 20vw;
  font-size: 15px;
  margin-bottom: 3vh;
  box-shadow: 0px 2px 4px 0px rgba(39, 39, 39, 0.1);
}

.city img {
  width: 11px;
  height: 16px;
  margin-right: 10px;
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

.city-popup {
  position: relative;
  overflow: hidden;
  color: #000;
  background-color: #fff;
  box-shadow: 0px 2px 10px 0px rgba(151, 151, 151, 0.2); 
  width: 40vw;
  height: 13vh;
  padding-left: 2.5%;
  padding-top: 28px;
  border-radius: 4px;
}
.city-popup_title{
  font-family: "FuturaPT";
  color: #000;
  font-size: 20px;
  font-weight: 600;
  line-height: 24px;
  letter-spacing: 0px;
}
.city-popup_list {
  font-family: "FuturaPT";
  display: flex;
  flex-direction: column;
  font-family: "FuturaPT";

}

.city-popup_input{
  width: 70%;
  padding: 12px 20px;
  margin: 8px 0;
  border-radius: 8px;
  box-sizing: border-box;
  border: 2px solid rgba(151, 151, 151, 0.5);
  cursor: pointer;
  outline: none;
}

.city-popup_button_disabled{
  font-family: "FuturaPT";
  cursor: not-allowed;
  color: rgba(39, 39, 39, 0.5);
  border: 2px solid rgba(151, 151, 151, 0.3);
  padding: 10px 20px;
  text-align: center;
  margin: 4px 2px;
  width: 22%;
  margin-left: 2%;
  border-radius: 30px;
  box-sizing: border-box;
  font-size: 16px;
}

.city-popup_button_active{
  font-family: "FuturaPT";
  cursor: pointer;
  color: #fff;
  border: none;
  background: linear-gradient(270deg, #FFA800 0%, #FF6F00 100%);
  box-shadow: 0px 5px 25px #FFA800;
  padding: 10px 20px;
  text-align: center;
  margin: 4px 2px;
  width: 22%;
  margin-left: 2%;
  border-radius: 30px;
  box-sizing: border-box;
  font-size: 16px;
}
</style>
