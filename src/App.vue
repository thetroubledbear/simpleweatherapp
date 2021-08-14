<template>
  <div id="nav">

    <h1>Weather <span>Vue App</span></h1>
  
  </div>

 <div class="home">
      <div class="search-city">
          <form @submit.prevent="searchCity()"> 
              <input type="text" placeholder="search" v-model="search">
              <input type="submit" value="search">
          </form>
      </div>
    <div class="weather-display" v-if= "typeof weatherdata.main != 'undefined'"> 
        <h1> {{weatherdata.name}}, {{weatherdata.sys.country}} </h1>
        <h2>{{Math.round(weatherdata.main.temp) }}°C</h2>
        <h2>{{weatherdata.main.humidity}} %</h2>
        <h3>{{weatherdata.weather[0].description}}</h3>
    </div>
  
    <div class="weather-icon"  v-if= "typeof weatherdata.main != 'undefined'">
        <img :src="require(`@/assets/icons/animated/${icon}.svg`)" alt="peng ting" width="300" height="300" />
    </div>
  </div>
</template>

<script>
import env from '@/env.js';

export default {
    data(){
        return {
            search: '',
            weatherdata: {},
            icon:''
        }
    },
    methods:{

        searchCity () { 
            if (this.search != ''){
                fetch(`https://api.openweathermap.org/data/2.5/weather?q=${this.search}&units=metric&APPID=${env.apikey}`)
                .then(response => {
                    return response.json();
                    })
                .then(this.setWeatherData)
            }
        },
        setWeatherData (results) { 
            this.weatherdata = results;
            this.icon = this.weatherdata.weather[0].icon
        },
       
        
    }


}
</script>



<style>
</style>

