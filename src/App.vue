<template>
  <div class="app__wrapper">
    <BasicDetailsVue 
      :icon="TodayWeather.weather[0].icon"
      :currTemp="TodayWeather.main.temp"
      :city="TodayWeather.name"
    />
    <WeatherPanelVue />
  </div>
</template>

<script>
import WeatherPanelVue from './components/WeatherPanel.vue';
import BasicDetailsVue from './components/BasicDetails.vue';
export default {
  name: 'App',
  components: {
    WeatherPanelVue,
    BasicDetailsVue
  },
  async created(){
    await fetch(`https://api.openweathermap.org/data/2.5/weather?q=Beijing&units=metric&appid=${process.env.VUE_APP_MY_KEY}`)
      .then(res => res.json())
      .then(json => {
        this.TodayWeather = json
      })
  },
  mounted(){
    console.log(this.TodayWeather)
  },
  data(){
    return{
      TodayWeather: {},
    }
  }
}
</script>

<style>

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
  text-align: center;
  color: white;
  /* margin-top: 60px; */
  background-image: url('./assets/Theme/clear_sky.jpg');
  box-sizing: border-box;
}
html, body{
  margin: 0;
  padding: 0;
}
</style>
