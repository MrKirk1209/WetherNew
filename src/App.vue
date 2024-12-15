<template>
  <div id="app">
  <h1>Weather by Me</h1>
<div class="card-now">
  <h2>Сейчас</h2>
  <p>Температура:{{ weatherData?.main.temp}}°C</p>
  <p>Город:{{ weatherData?.name}}</p>
  <p>Давление:{{ weatherData?.main.pressure}}мм рт. ст</p>
  <p>Влажность:{{ weatherData?.main.humidity}}%</p>
  <p>Ветер:{{ weatherData?.wind.speed}}м/c</p>
  <p>Погода:{{ weatherData?.weather[0].description}}</p>
  <div class="img-now"><img :src="require(`@/assets/${idIconImage}`)" alt="" v-if="idIconImage"></div>
  

  </div>
  <select v-model="selectedCity" @change="getWeatherData(selectedCity)">
  <option>Нижний Тагил</option>
  <option>Рига</option>
  <option>Берн</option>
  </select>
  
    
  <div v-if="forecastData" class="body-card__app">
    <ForecastCard 
    v-for="(f,i) in forecastData.list"
    :key="i"
    :weatherData="f"
    :idIconImage="getWeatherIconForcast(i)"></ForecastCard>
  </div>
  </div>
</template>

<script>
import ForecastCard from './components/ForecastCard.vue';

export default {
  name: 'App',
  data() {
    return {
      weatherData: null,
      apiId:null,
      idIconImage:null,
      forecastData:null,
      сoordinatCity:
    {
      "Нижний Тагил":[57.9194,59.965],
      "Рига":[56.946,24.1059],
      "Берн":[46.947978,7.440386],
    },
    idIcon:{
      200: "11d.png", 	
      201: "11d.png", 	 
      202: "11d.png", 	 
      210: "11d.png",
      211: "11d.png", 	
      212: "11d.png",
      221: "11d.png", 
      230: "11d.png", 	 
      231: "11d.png", 	 
      232: "11d.png",
      300: "09d.png", 	
      301: "09d.png", 	
      302: "09d.png", 	
      310: "09d.png", 	
      311: "09d.png", 	 	
      312: "09d.png", 	
      313: "09d.png", 	
      314: "09d.png", 	 
      321: "09d.png",
      500: "10d.png", 
      501: "10d.png", 
      502: "10d.png",  
      503: "10d.png",  	
      504: "10d.png",
      511: "10d.png", 
      520: "10d.png",  
      521: "10d.png", 
      522: "10d.png",  
      531: "10d.png",
      600: "13d.png",
      601: "13d.png", 	
      602: "13d.png", 	
      611: "13d.png", 	
      612: "13d.png", 	
      613: "13d.png", 
      615: "13d.png", 	
      616: "13d.png", 	
      620: "13d.png", 
      621: "13d.png", 	
      622: "13d.png", 	
      701:"50d.png", 	
      711:"50d.png", 	
      721:"50d.png", 	
      731:"50d.png", 	
      741:"50d.png", 
      751:"50d.png", 	
      761:"50d.png", 	
      762:"50d.png", 	
      771:"50d.png", 	
      781:"50d.png", 	
      800:"01d.png", 	
      801:"02d.png", 	
      802:"03d.png", 	
      803:"04d.png", 	
      804:"04d.png"
      },
    selectedCity:'Нижний Тагил',
    }
  
  },
      // fetch("test.json")
      // fetch("forecast.json")
mounted() {
  fetch('https://api.openweathermap.org/data/2.5/weather?lat=57.9194&lon=59.965&appid=746d83f9add043d69ce9d81746dc3dc9&units=metric&lang=ru')

      .then(data => data.json())
      .then(data => {this.weatherData = data;
      this.getWeatherIcon();
      });
    fetch('https://api.openweathermap.org/data/2.5/forecast?lat=57.9194&lon=59.965&appid=746d83f9add043d69ce9d81746dc3dc9&units=metric&lang=ru')

      .then(data => data.json())
      .then(data => {this.forecastData = data;
      this.getWeatherIcon();
      });

  },
  methods:{
        getWeatherData() {
      fetch(`https://api.openweathermap.org/data/2.5/weather?lat=${this.сoordinatCity[this.selectedCity][0]}&lon=${this.сoordinatCity[this.selectedCity][1]}&appid=746d83f9add043d69ce9d81746dc3dc9&units=metric&lang=ru`)
      .then(resp => resp.json())
      .then(data =>{this.weatherData = data;});
      
      fetch(`https://api.openweathermap.org/data/2.5/forecast?lat=${this.сoordinatCity[this.selectedCity][0]}&lon=${this.сoordinatCity[this.selectedCity][1]}&appid=746d83f9add043d69ce9d81746dc3dc9&units=metric&lang=ru`)
      .then(data => data.json())
      .then(data => {this.forecastData = data;});
    },
            getWeatherIconForcast(id) {
        const weathericonid=this.forecastData.list[id].weather[0].id
        const icon=this.idIcon[weathericonid]
        return icon

    },
        getWeatherIcon() {
        this.apiId=this.weatherData.weather[0].id
        this.idIconImage=this.idIcon[this.apiId]
        const icon=this.idIcon[this.apiId]
        return icon

    },

  },
  components: {
    ForecastCard
  }
}
</script>

<style>
#app {
  text-align: center;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;

}
.body-card__app{
  margin-top: 30px;
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 30px;
}
.card-now{
  display: flex;
  justify-content: center;
  flex-direction: column;
  border: 1px solid black;
  padding: 5px;
}
</style>
