<template>
  <!-- <img alt="Vue logo" src="./assets/logo.png"> -->
  <!-- <HelloWorld msg="Hi TJ"/> -->
  <div class="app__wrapper">
    <h1 class="Header">This is the Header</h1>
    <ul class="list__container">
      <li class="list__item" v-for="item in items" v-bind:key="item.id">
        {{ item.name }}
      </li>
    </ul>
  </div>
</template>

<script>
// import HelloWorld from './components/HelloWorld.vue'

export default {
  name: 'App',
  components: {
    
  },
  mounted(){
    console.log(process.env.VUE_APP_MY_KEY)
    
    fetch(`https://api.openweathermap.org/data/2.5/weather?q=Beijing&appid=${process.env.VUE_APP_MY_KEY}`)
      .then(res => res.json())
      .then(json => console.log(json))

  },
  data(){
    return{
      items: []
    }
  }
}
</script>

<style>

.app__wrapper{
  width: 100%;
  min-height: 100vh;
  background-color:#2c3e5057;
}
.list__container{
  display: flex;
  flex-direction: column;
  align-items: center;
  margin:0;
  padding: 0;
}
.list__item{
  width: 70%;
  height: 8rem;
  border-radius: 10px 10px;
  background-color: whitesmoke;
  list-style: none;
  margin: 2rem 0;
  color: black;
}
 h1 { 
  margin: 0;
  padding: 0;
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
