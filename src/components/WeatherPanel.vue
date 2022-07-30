<script>
import SearchBarVue from './SearchBar.vue';
import AttributeRow from './AttributeRow.vue';

    export default {
        props:{
            Weather: Object,
            showPanel: Boolean
        },
        data(){
            return{
                feelsLike: this.Weather.main.temp,
                maxTemp: this.Weather.main.temp_max,
                minTemp: this.Weather.main.temp_min,
                humidity: this.Weather.main.humidity,
                pressure: this.Weather.main.pressure,
                windSpeed: this.Weather.wind.speed,
                windDeg: this.Weather.wind.deg,
                cloudy: this.Weather.clouds.all
            }
        },
        components:{
            SearchBarVue,
            AttributeRow,
        },
        mounted(){
            console.log(this.Weather)
        }
    }
</script>

<template>
    <div :class="showPanel ? 'panel__container show_panel' : 'panel__container hide_panel' ">
        <SearchBarVue />

        <div class="panel__details__wrapper">
            <h3 class="panel__details__title">Weather Details</h3>
            <AttributeRow title="Feels like" :value="Math.round(feelsLike) + '&#176;'"/>
            <AttributeRow title="Max-temperature" :value="Math.round(maxTemp) + '&#176;'" />
            <AttributeRow title="Min-temperature" :value="Math.round(minTemp) + '&#176;'" />
        </div>  

        <div class="panel__details__wrapper">
            <AttributeRow title="Cloudy" :value="cloudy + '%'" />
            <AttributeRow title="Humidity" :value="humidity + '%'" />
            <AttributeRow title="Wind Speed" :value="Math.round(windSpeed) + 'm/s'" />
            <AttributeRow title="Wind Direction" :value="windDeg + '&#176;'" />
            <AttributeRow title="Pressure" :value="pressure + 'hPa'" />
        </div>
    </div>
</template>

<style>
    .panel__container{
        width: 40%;
        height: 100%;
        overflow-y: scroll;
        background-color: rgba(0, 0, 0, 0.201);
        backdrop-filter: blur(10px);
        position: relative;
        padding: 0 3rem;
    }

    @media (max-width: 920px){
        .panel__container{
            width: unset;
        }

        .show_panel{
            animation: showPanel;
            animation-duration: 1s;
            animation-fill-mode:forwards;
        }

        .hide_panel{
            animation: hidePanel;
            animation-duration: 1s;
            animation-fill-mode:forwards;
        }

        @keyframes showPanel {
            from{
                transform: translateY(0)
            }
            to{
                transform: translateY(-100%)
            }
        }

        @keyframes hidePanel {
            from{
                transform: translateY(-100%)
            }
            to{
                transform: translateY(0)
            }
            
        }

    }

    .panel__details__wrapper{
        width: 100%;
        border-bottom: 1px solid gray;
    }

    .panel__details__title{
        text-align: start;
    }
</style>