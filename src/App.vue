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
      const response = await fetch(
        `https://api.openweathermap.org/data/2.5/weather?q=${this.getCityName}&appid=0d6a6af851440ceceedf74e5105cbdc3`
      );
      const data = await response.json();
      this.cityData = data;

        if(this.cityDataList.length<4){
        this.cityDataList.unshift(this.cityData);
        console.log(this.cityDataList);
      }
      if (this.cityDataList.length==4){
        this.cityDataList.splice(3,1);
        this.cityDataList.unshift(this.cityData);
      }
      
      this.localCards = localStorage.setItem('cardInfo',JSON.stringify(this.cityDataList));
      this.getCityName="";
     
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
  width: 100%;
  height: 10vh;
  background-color: rgb(233, 238, 193);
}

#header-content {
  display: flex;
  justify-content: space-around;
  width: 20%;
}
#header-content_input {
  width: 15rem;
  margin-right: 5px;
  background-color: rgb(255, 255, 255);
}

#header-content_button {
  width: 4rem;
  border-style: none;
  box-shadow: 3px 4px 6px #888888;
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
  background-color: rgb(199, 227, 240);
}
</style>
