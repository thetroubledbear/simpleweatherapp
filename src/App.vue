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
        <div class="wname">
            <h1>{{weatherdata.name}}, {{weatherdata.sys.country}} </h1>
        </div>
        <div class="wicon">
            <img :src="require(`@/assets/icons/animated/${icon}.svg`)" alt="weather icon" width="150" height="150" />
        </div>
        <div class="wtemp">
            <h2>{{Math.round(weatherdata.main.temp) }}°C</h2>
        </div>
        <div class="wdesc">
            <h2>{{weatherdata.weather[0].description}}</h2>
        </div>
        <div class="whum">
            <h2>Humidity: {{weatherdata.main.humidity}}%</h2>
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
    --prmcolor: #97c2e4; 
    --sndcolor: #D0FFD6;
    --bgelcolor:  #5A7D7C;
    --btncolor : #5A7D7C;
    --generealbg: #64839b; 
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
        padding-bottom: 5px;
        border-bottom: 5px solid var(--prmcolor);
    }
}
.search-city { 
    display: flex;
    align-content: center;
    justify-content: center;
    padding-bottom: 20px;

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
        color: white;
        margin-left: 5px;
        border: none;
        outline: none;
        background-color: var(--btncolor);
        padding: 10px 20px;
        border-radius: 99px;

        font-size: 15px;
        font-weight: 300;
        text-transform: uppercase;
        cursor: pointer;
        transition: 0.3s;

        &:hover{
            background-color: var(--prmcolor);
        }
    }

}

.weather-display {
    
    width: 300px;
    border: 5px solid var(--prmcolor);
    border-radius: 15px 50px;
    padding: 20px 32px;
    padding-top: 5px;
    margin: auto;
    box-shadow: 0 5px 8px 0 rgba(0, 0, 0, 0.2), 0 7px 20px 0 rgba(0, 0, 0, 0.19);
    background-color: var(--bgelcolor);
    color: white;
    
    .wicon{
        position: relative;
        margin: auto;
        width: 70%;
        
    }

    .wname{
        font-size: 10px;
        padding-top: 7px;
    }

    .wtemp{
        margin-top: -20px;
        font-size: 20px;
        text-align: center;
        margin-bottom: 4px;
    }
    .wdesc{
        font-size: 10px;
        text-transform: uppercase;
        text-align: center;
        margin-bottom: 5px;
    }
    .whum{
        padding-top: 5px;
        font-size: 10px;
        text-align: center;
    }


}


</style>

