<template>
  <div 
    v-if="is_data_fetched" 
    class="app__container" 
    :style="{backgroundImage: `url(${getBackgroundTheme})`}"
  >
    <div class="app__wrapper">
      <BasicDetailsVue 
        :icon="TodayWeather.weather[0].icon"
        :currTemp="TodayWeather.main.temp"
        :city="TodayWeather.name"
      />
      <WeatherPanelVue />
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
      )
  },
  mounted(){
    console.log('mounted')
  },
  computed:{
    getBackgroundTheme(){
        const code = this.TodayWeather.weather[0].id;
        let theme;
    console.log(code);
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
      backgroundTheme: null
    }
  }
}
</script>

<style>
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
