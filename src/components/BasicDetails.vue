<script>
    export default {
        props: {
            icon: String,
            currTemp: Number,
            city: String
        },
        data(){
            return{
                month: '',
                day: '',
                date: '',
                time: '',
                year: ''
            }
        },
        mounted(){
            console.log(this.currTemp)
            this.getEntireDate();
        },
        methods:{
            getEntireDate(){
                const months = ["Jan", "Feb", "Mar", "Apr", "May", "Jun", "Jul", "Aug", "Sep", "Oct", "Nov", "Dec"];
                const days = ["Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday", "Sunday"];

                const d = new Date();
                this.day = days[d.getDay()];
                this.month = months[d.getMonth()]; 
                this.date = d.getDate();
                this.year = d.getFullYear();

            },
        }
    }
</script>

<template>
    <div class="basic__details__container">
        <div class="basic__details__wrapper">
            <h1> {{ Math.round(currTemp) }}&#176;</h1>
            <div class="basic__details__location">
                <h2> {{ city }} </h2>
                <small>{{ day }}, {{ date }} {{month}} {{ year }}</small>
            </div>
            <img :src="'http://openweathermap.org/img/wn/' + icon + '@2x.png'" alt="icon" />
        </div>
    </div>
</template>

<style scoped>
    h1{
        font-size: clamp(1.5rem, 7vw, 5rem);
        margin-right: 1.5rem;
    }

    h2{
        font-size: clamp(1rem, 4vw, 4rem);
        font-weight: 500;
        margin: 0;
    }
    .basic__details__location{
        display: flex;
        flex-direction: column;
    }
    .basic__details__container{
        width: 60%;
        height: 100vh;
    }

    .basic__details__wrapper{
        position: absolute;
        display: flex;
        flex-direction: row;
        align-items: center;
        bottom: 3rem;
        left: 3rem;
        color: white;
        /* width: 80%; */
        justify-content: space-evenly;
    }

    @media (max-width: 920px){
        .basic__details__container{
            width: 100%;
        }

        .basic__details__wrapper{
            top: 0;
            left: 0;
            right: 0;
            margin: 0 auto;
        }
    }
</style>