<template>
<div class="card-container">
    <div :id="windEffect()" >
    <div id="card">
        
        <div :class= "genereteBackground()">
             
            <div id="cityName">
              <h3>{{cityData.name}}</h3>
              <h6> {{getCountryName}}</h6>
            </div>
            <div id="card-temp">
              <img :src="getWeatherIcon" alt="" id="card-temp-icon" >
              <h1 id="card-temp-info" v-if="formatTemp">{{formatTemp}}°C</h1>
              <h1 id="card-temp-info" v-else></h1>
            </div>
            <div id="weatherCondition">
              <p>{{weatherCondition}}</p>
              <p></p>
            </div> 
            <div id="weatherInfoFrame-content">
                <p v-if="weatherMaxTemperature">H:{{weatherMaxTemperature}}°C</p>
                <p v-else></p>
                <p v-if="weatherMinTemperature">L:{{weatherMinTemperature}}°C</p>
                <p v-else></p>
                <p v-if="windSpeed">Wind Speed: <span>{{windSpeed}} mps</span></p>
                <p v-else></p>

            </div>
            <div id="weatherInfoFrame"></div>
        </div>
      </div>
    </div>
 </div>  
</template>

<script>
 export default{

     props:{         //['cirtyData']
        cityData:{},
        cityDataList:[]
    },
    methods:{

    genereteBackground(){
      
      let iconId = this.cityData.weather?.[0]?.id;
      console.log(iconId);
      if(iconId >= 500 && iconId<= 531)
        return "card-content weather_rain";
      else if (iconId >= 701 && iconId <= 781)
        return "card-content weather_mist";
      else if (iconId === 800)
        return "card-content weather_clear";
      else if (iconId === 801)
        return ("card-content weather_fewClouds");
      else if (iconId == 802)
        return ("card-content weather_scatteredClouds");
      else if(iconId >= 803)
        return "card-content weather_darkClouds";
    },

    windEffect(){
        if((this.cityData.wind?.speed)>=6)
        return "shaken";
        else
        return "nonshake";
    },
},


computed: {
     formatTemp(){
        return (Math.round(this.cityData?.main?.temp - 272));
      },
      weatherCondition(){
        return this.cityData.weather?.[0]?.description;
      },
      getWeatherIcon(){
        let iconNumber = this.cityData.weather?.[0]?.icon;
        let icon=`http://openweathermap.org/img/wn/${iconNumber}@2x.png`;
        return icon;
      },
      weatherMinTemperature(){
       return (Math.round(this.cityData?.main?.temp_min - 272));
      },
      weatherMaxTemperature(){
        return (Math.round(this.cityData?.main?.temp_max - 272));
      },
        windSpeed(){
        return (this.cityData?.wind?.speed);
      },

    getCountryName(){
        return (this.cityData?.sys?.country);
    }
     
  },

 }
</script>

  
<style>
.card-container{
  height: 420px;
}
#deleteButton{
background-color: #888888;
border-style: none;
}
#card{
  width: 100%;
  height: 65vh;
  display: flex;
  align-items: center;
  margin: 2rem;
}

.card-content{
  display: flex;
  justify-content: space-around;
  align-items: center;
  flex-direction: column;
  width: 260px;
  height: 400px;
  box-shadow:3px 4px 6px #888888;
  border-radius: 15px;
  }
.weather_clear{
    background: linear-gradient(330deg, rgb(201 233 242), rgb(196 238 247),rgb(207 231 237),rgb(255 235 59 / 52%));
}
.weather_rain{
  background: linear-gradient(-40deg, rgb(174, 179, 187),rgb(186, 190, 199),rgb(247, 221, 174));
}
.weather_mist{
  background: linear-gradient(-40deg, rgb(174, 179, 187),rgb(186, 190, 199),rgb(247, 221, 174));
}
.weather_fewClouds{
  background: linear-gradient(-45deg, rgb(200, 214, 235), rgb(213, 232, 244),rgb(227, 230, 234),rgb(245, 225, 157));
}
.weather_scatteredClouds{
  background: linear-gradient(-49deg, rgb(183, 235, 250),rgb(240, 246, 247), rgb(164, 216, 240),rgb(245, 211, 157));
}
.weather_darkClouds{
   background: linear-gradient(-45deg, rgb(200, 214, 235), rgb(220, 242, 255),rgb(194, 203, 212),rgb(245, 211, 157));
}

#cityName{
display: flex;
align-items: center;
justify-content: center;
font-size: xx-large;
height: 8vh;
margin:2rem;
}

#cityName h6{
    font-size:small;
    margin-left:0.5rem;
    position:
}

#card-temp{
  display:flex;
  align-items: center;
  height: 3vh;
  width: 250px;
  margin-top: 1rem;
}

#card-temp-icon{
  width: 140px;
  margin-right: 1rem;
}

#card-temp-info{
  font-family: 'Lucida Sans', 'Lucida Sans Regular', 'Lucida Grande', 'Lucida Sans Unicode', Geneva, Verdana, sans-serif;
  font-size: 50px;
  margin-right: 2rem;
  
}

#weatherCondition{
    margin-top:3rem;
    height: 12vh;
}

#weatherInfoFrame-content{
  z-index: 110;
  margin-bottom: 1.2rem;
}
#weatherInfoFrame-content p{
    margin:1rem;
}

#weatherInfoFrame{
  background-color: aliceblue;
  width: 13rem;
  height: 15vh;
  opacity:0.5;
  z-index: 10;
  position:absolute;
  margin-top: 15rem;

}

#shaken :hover{
    animation: shake 1.2s;
    animation-iteration-count: infinite;
}

@keyframes shake {
    0% { transform: translate(1px, 1px) rotate(0deg); }
    10% { transform: translate(-1px, -2px) rotate(-1deg); }
    20% { transform: translate(-3px, 0px) rotate(1deg); }
    30% { transform: translate(3px, 2px) rotate(0deg); }
    40% { transform: translate(1px, -1px) rotate(1deg); }
    50% { transform: translate(-1px, 2px) rotate(-1deg); }
    60% { transform: translate(-3px, 1px) rotate(0deg); }
    70% { transform: translate(3px, 1px) rotate(-1deg); }
    80% { transform: translate(-1px, -1px) rotate(1deg); }
    90% { transform: translate(1px, 2px) rotate(0deg); }
    100% { transform: translate(1px, -2px) rotate(-1deg); }
  }
  @media only screen and (max-width: 390px) {
#card{
  width: 100%;
  height: 65vh;
  display: flex;
  align-items: center;
  margin: 0;
}
}

</style>