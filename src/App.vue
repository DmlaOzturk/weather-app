<template>
  <div id="weatherApp">
    <div id="header">
      <div id="header-content">
        <input
          id="header-content_input"
          type="text"
          placeholder="Please city name"
          v-model="getCityName"
          @keyup.enter="getWeatherByName()"
        />
        <button
          id="header-content_button"
          @click="getWeatherByName()"
        
        >
          submit
        </button>
      </div>
      
    </div>
    <div id="card-list">
   
      <div v-for= "(cityData, index) in cityDataList" :key= "index">
       <weatherCards :cityData="cityData"/> 
       </div>
    </div>
    <div id="footer">
    </div>
  </div>
</template>

<script>

import WeatherCards from "./components/WeatherCards.vue"

export default {
  data() {
    return {
      getCityName: "",
      cityData: {},
      cityDataList:[],
      localCards:[],
    };
  },

  methods:{
    async getWeatherByName() {
      if(this.getCityName !==""){
      const response = await fetch(
        `https://api.openweathermap.org/data/2.5/weather?q=${this.getCityName}&appid=0d6a6af851440ceceedf74e5105cbdc3`
      );
      const data = await response.json();
      this.cityData = data;

        if(this.cityDataList.length<5){
        this.cityDataList.unshift(this.cityData);
      }
      if (this.cityDataList.length >=5){
        this.cityDataList.splice(4,1);
      }
      
      this.localCards = localStorage.setItem('cardInfo',JSON.stringify(this.cityDataList));
      this.getCityName="";
    }
    },
    mounted(){
      if (localStorage.getItem('cardInfo')){
        this.localCards = JSON.parse(localStorage.getItem('cardInfo'));
      }
    }
   
  },
  
  components:{
    WeatherCards
    }
};
</script>

<style>

html,
body {
  margin: 0;
  padding: 0;
   overflow:hidden;
}
#weatherApp {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin: 0;
  padding: 0;
  height: 100vh;
  width: 100%;
 
}

#header {
  display: flex;
  justify-content: center;
  align-items: center;
  margin: 0;
  padding: 0;
  width: 100%;
  height: 10vh;
  background-color: rgb(211, 228, 231);
}

#header-content {
  display: flex;
  justify-content: space-around;
  width: 40%;
}
#header-content_input {
  width: 25rem;
  border-style: none;
  height: 2rem;
  margin-right: 10px;
  background-color: rgb(255, 255, 255);
}

#header-content_button {
  width: 7rem;
  border-style: none;
  box-shadow: 3px 4px 6px #888888;
  border-radius: 3rem;
  color: rgb(15, 18, 20);
  background-color: aliceblue;
}
#header-content_button:hover {
  transform: scale(1.1);
  cursor: pointer;
}

#card-list {
  display: flex;
  align-items: center;
  justify-content: center;
  height: 80vh;
}
#footer {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
  height: 10vh;
}
#header-content_button:active{
  color: rgb(96, 94, 232);
}
#header-content_input:focus{
  outline: none !important;
  background-color: rgb(255, 255, 255);
}
</style>
