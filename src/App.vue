<template>
  <div class="city text" @click="showPopup = true">
    <img src="../src/assets/geoposition.png">
    {{ cityName }}
  </div>
  <router-view v-if="reranderComponents" />
  <div v-show="showPopup" class="city-popup_container">
    <div @click="showPopup = false" class="city-popup_mask"></div>
    <div class="city-popup">
      <span class="city-popup_title text">Выбор населённого пункта:</span>
      <img class="city-popup_close" @click="showPopup = false" src="../src/assets/close.png">
      <div class="city-popup_search">
        <div class="city-popup_input_container" :class="{ 'city-popup_input_container_active': showCities }">
          <div class="input_group" style="position: relative !important;">
            <input type="text" v-model="searchInput" @input="getCity" placeholder="Например, Санкт-петербург"
              class="city-popup_input text" :class="{ 'city-popup_input_actie': showCities }">
            <span class="clear_button" @click="searchInput = ''; showCities= false"><img src="../src/assets/close.png"></span>
          </div>
          <div v-if="showCities" class="city-popup_list text">
            <span style="padding: .5em;" v-for="city in cityArr" :key="city.id"
              @click="chooseCity(city.id, city.city, city.label)">{{ city.label }}</span>
          </div>
        </div>
        <span @click="changeCity()"
          :class="buttonActive ? 'city-popup_button_active' : 'city-popup_button_disabled'" class="text">Подтверить</span>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      cityArr: [],
      cityName: '',
      searchInput: '',
      id: 0,
      showPopup: false,
      buttonActive: false,
      showCities: false,
      reranderComponents: true,
    }
  },
  methods: {
    async getCity() {
      if (this.searchInput.length >= 3) {
        this.showCities = true;
        try {
          let res = await axios.get(`https://nlstar.com/api/catalog3/v1/city/?country=ru&term=${this.searchInput}`)
          this.cityArr = res.data.data;
        } catch (error) {
          alert(error.message);
        }
      }
      else {
        this.showCities = false;
      }
    },
    changeCity() {
      if (this.searchInput.length >= 3) {
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
      this.showCities = false;
    },
    loadMenu() {
      this.reranderComponents = false;
      this.reranderComponents = true;
    },
  },
  mounted() {
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

.text{
  font-family: "FuturaPT";
}

.city {
  display: flex;
  align-items: center;
  height: 7vh;
  width: 100vw;
  box-sizing: border-box;
  padding-left: 20vw;
  font-size: 1rem;
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

.city-popup_container {
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
  display: flex;
  flex-direction: column;
  color: #000;
  background-color: #fff;
  box-shadow: 0px 2px 10px 0px rgba(151, 151, 151, 0.2);
  width: 40vw;
  height: 144px;
  padding-left: 2.5%;
  padding-top: 28px;
  border-radius: 4px;
}

.city-popup_title {
  font-family: "FuturaPT";
  color: #000;
  font-size: 1.4rem;
  font-weight: 600;
  line-height: 24px;
  letter-spacing: 0px;
}

.city-popup_close {
  position: absolute;
  right: 1em;
  top: 1em;
  width: 1em;
  height: 1em;
  cursor: pointer;
}

.city-popup_search {
  margin-top: 1em;
  display: flex;
}

.city-popup_input_container {
  display: flex;
  flex-direction: column;
  box-sizing: border-box;
  width: 70%;
  padding: 0;
  border-radius: 8px;
}

.city-popup_input_container_active {
  border: 2px solid rgba(39, 39, 39, 1);
}

.clear_button {
  position: absolute;
  top: 50%;
  right: 0;
  transform: translate(-50%, -50%);
  margin-right: 20px;
  color: #979797;
  cursor: pointer;
  transition: 0.3s ease-in-out;
}

.clear_button img{
  width: 12px;
  height: 12px;
}
.city-popup_input {
  width: 100%;
  padding: 12px 20px;
  border-radius: 8px;
  font-size: 1rem;
  box-sizing: border-box;
  border: 2px solid rgba(151, 151, 151, 0.5);
  cursor: pointer;
  outline: none;
  height: 100%;
}

.city-popup_input_actie {
  border: 2px solid #fff;
  border-bottom: 2px solid rgba(151, 151, 151, 0.5);
  border-bottom-left-radius: 0 !important;
  border-bottom-right-radius: 0 !important;
}
.city-popup_list {
  display: flex;
  flex-direction: column;
  font-family: "FuturaPT";
  background-color: #fff;
  padding: 12px 20px;
  font-size: 1rem;
  border-radius: 8px;
  overflow-y: scroll;
  box-sizing: border-box;
  cursor: pointer;
  margin-top: 0;
  max-height: 30vh;
}

.city-popup_button_disabled {
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
  display: block;
  font-size: 1rem;
  height: 48px;
}

.city-popup_button_active {
  display: block;
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
  font-size: 1rem;
}
</style>
