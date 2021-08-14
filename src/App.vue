<template>
 <div class="home">
    <div class="title">
        <h1>Weather <span>Vue App</span></h1>
    </div>

    <div class="search-city">
          <form @submit.prevent="searchCity()"> 
              <input type="text" placeholder="search" v-model="search">
              <input type="submit" value="GO">
          </form>
    </div>
    <div class="weather-display" v-if= "typeof weatherdata.main != 'undefined'"> 
        <div class="wicon">
            <img :src="require(`@/assets/icons/animated/${icon}.svg`)" alt="weather icon" width="200" height="200" />
        </div>
        <div class="wname">
            <h1>{{weatherdata.name}}, {{weatherdata.sys.country}} </h1>
        </div>
        <div class="wtemp">
            <h2>{{Math.round(weatherdata.main.temp) }}°C</h2>
        </div>
        <div class="whum">
            <h2>{{weatherdata.main.humidity}} %</h2>
        </div>
        <div class="wdesc">
            <h3>{{weatherdata.weather[0].description}}</h3>
        </div>
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
            icon:'',
           
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



<style lang="scss">
:root {
    --prmcolor: aquamarine; 
    --sndcolor: rgb(54, 114, 94); 
}
*{
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    font-family: 'Fira Sans', sans-serif;
}

.title{
    padding: 24px 32px;
    display: flex;
    justify-content: center;

    h1{
        font-size: 30px;
        color: black;
        font-weight: 300;
        text-align: center;
        text-transform: uppercase;
        padding-bottom: 10px;
        border-bottom: 5px solid var(--prmcolor);
    }
}
.search-city { 
    display: flex;
    align-content: center;
    justify-content: center;

    input[type="text"] {
        border-radius: 10px;
        text-align: center;
        border: none;
        outline: none;
        padding: 10px 20px;
        border-radius: 99px;
        font-size: 15px;
        font-weight: 300;
        transition: 0.3s;
        text-transform: uppercase;
          &:hover{
            background-color: var(--prmcolor);
        }
    }
    input[type="submit"]{
        border: none;
        outline: none;
        background-color: var(--prmcolor);
        padding: 10px 20px;
        border-radius: 99px;

        font-size: 15px;
        font-weight: 300;
        text-transform: uppercase;
        cursor: pointer;
        transition: 0.3s;

        &:hover{
            background-color: var(--sndcolor);
        }
    }

}

.weather-display {
    position: relative;
    margin: 0 auto;
    padding: 32px;
    max-width: 700px; 

}


</style>

