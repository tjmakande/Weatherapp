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
      <WeatherPanelVue :Weather="TodayWeather" :showPanel="isPanelVisible"/>

      <div :class="isPanelVisible ? 'mobile__arrow__container transparent_backdrop' : 'mobile__arrow__container'">
        <div class="mobile__arrow__wrapper" @click="isPanelVisible = !isPanelVisible">
          <p id="details__text">{{isPanelVisible ? 'Hide Details': 'View Details'}}</p>
          <svg 
            xmlns="http://www.w3.org/2000/svg" 
            viewBox="0 0 320 512"
            :class="isPanelVisible ? 'arrow turned' : 'arrow' "
          >
            <path 
              d="M151.5 427.8L3.5 281c-4.7-4.7-4.7-12.3 0-17l19.8-19.8c4.7-4.7 12.3-4.7 17 0L160 362.7l119.7-118.5c4.7-4.7 12.3-4.7 17 0l19.8 19.8c4.7 4.7 4.7 12.3 0 17l-148 146.8c-4.7 4.7-12.3 4.7-17 0zm17-160l148-146.8c4.7-4.7 4.7-12.3 0-17l-19.8-19.8c-4.7-4.7-12.3-4.7-17 0L160 202.7 40.3 84.2c-4.7-4.7-12.3-4.7-17 0L3.5 104c-4.7 4.7-4.7 12.3 0 17l148 146.8c4.7 4.7 12.3 4.7 17 0z"
              fill="white"
            />
          </svg>
        </div>
      </div>
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
          return theme;
    }
  },
  data(){
    return{
      TodayWeather: null,
      is_data_fetched: false,
      failed_to_fetch: false,
      backgroundTheme: null,
      isPanelVisible: false,
    }
  }
}
</script>

<style>

#details__text{
  color: white;
  font-size: clamp(1rem, 2.5vw, 2rem);
  margin-top: 0;
}
.arrow{
  width: clamp(1rem, 2.8vw, 3rem);
  margin-top: 0;
  transform: rotate(180deg);
}
.turned{
  transform: rotate(0deg);
}

.mobile__arrow__container{
  position: absolute;
  bottom: 0;
  width: 100%;
  height: 10rem;
  background-color: rgba(0, 0, 0, 0.201);
  backdrop-filter: blur(5px);
  display: none;
  transition: .5s;
}
.transparent_backdrop{
  background: transparent;
  backdrop-filter: unset;
}

.mobile__arrow__wrapper{
  height: 100%;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  width: 12rem;
  margin: 5px auto;
  cursor: pointer;
}

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
 overflow: hidden;
}

.app__wrapper{
  width: 100%;
  min-height: 100vh;
  display: flex;
  flex-direction: row;
  background-color:#2c3e5057;
}

@media (max-width: 920px){
  .app__container{
    height: 100vh;
  }
  .app__wrapper{
    flex-direction: column;
  }

  .mobile__arrow__container{
    display: block;
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
