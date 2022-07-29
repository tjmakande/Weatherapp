<template>
  <div 
    class="app__container" 
    v-if="is_data_fetched"
    :style="{backgroundImage: `url(${getBackgroundTheme})`}"
  >
    <div class="app__wrapper">
      <BasicDetailsVue 
        :icon="TodayWeather.weather[0].icon"
        :currTemp="TodayWeather.main.temp"
        :city="TodayWeather.name"
      />
      <WeatherPanelVue :Weather="TodayWeather"/>
    </div>
  </div>
  <div class="error__container" v-if="failed_to_fetch" :style="{backgroundImage: `url(${errorBG})`, backgroundSize: 'cover'}">
    <div class="error__wrapper">
      <h3 >Unable to load the data. Please try again later...</h3>
    </div>
  </div>
</template>

<script>
import WeatherPanelVue from './components/WeatherPanel.vue';
import BasicDetailsVue from './components/BasicDetails.vue';

import brokenclouds from './assets/Theme/brokenclouds.jpg';
import clearsky from './assets/Theme/clearsky.jpg';
import fewclouds from './assets/Theme/fewclouds.jpg';
import mist from './assets/Theme/mist.jpg';
import rain from './assets/Theme/rain.jpg';
// import scatteredclouds from './assets/Theme/scatteredclouds.jpg';
import showerrain from './assets/Theme/showerrain.jpg';
import snow from './assets/Theme/snow.jpg';
import thunderstorm from './assets/Theme/thunderstorm.jpg';

export default {
  name: 'App',
  components: {
    WeatherPanelVue,
    BasicDetailsVue,
  },
  async created(){
    console.log('created')
    await fetch(`https://api.openweathermap.org/data/2.5/weather?q=nuernberg&units=metric&appid=${process.env.VUE_APP_MY_KEY}`)
      .then(res => res.json())
      .then(json => {
        this.TodayWeather = json;
      })
      .then(() => 
        this.is_data_fetched = true
      ).catch(() => 
        this.failed_to_fetch = true
      )
  },
  mounted(){
    console.log('mounted')
  },
  computed:{
    errorBG(){
      return clearsky;
    },
    getBackgroundTheme(){
        const code = this.TodayWeather.weather[0].id;
        let theme;
        switch(true) {
          case (code < 250): theme = thunderstorm;
            break;
          case (code < 330): theme = showerrain;
            break;
          case (code < 550): theme = rain;
            break;
          case (code < 630): theme = snow;
            break;
          case (code < 790): theme = mist;
            break;
          case (code === 800): theme = clearsky;
            break;
          case (code <= 802): theme = fewclouds;
            break;
          case(code <= 804): theme = brokenclouds;
            break;
          default: theme = clearsky
        }
        console.log(theme);
          return theme;
    }
  },
  data(){
    return{
      TodayWeather: null,
      is_data_fetched: false,
      failed_to_fetch: false,
      backgroundTheme: null
    }
  }
}
</script>

<style>

.error__container{
  height: 100vh;
}

.error__wrapper{
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.701);
  backdrop-filter: blur(2px);
  display: flex;
  justify-content: center;
  align-items: center;
  color: white;
}

.app__container{
 text-align: center;
 color: white;
 background-size: cover;
 box-sizing: border-box;
}
.app__wrapper{
  width: 100%;
  min-height: 100vh;
  display: flex;
  flex-direction: row;
  background-color:#2c3e5057;
}

@media (max-width: 800px){
  .app__wrapper{
    flex-direction: column;
  }
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
html, body{
  margin: 0;
  padding: 0;
}
</style>
