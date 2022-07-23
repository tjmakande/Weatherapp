<script>
import SearchBarVue from './SearchBar.vue';

    export default {
        data(){
            return{

            }
        },
        components:{
            SearchBarVue
        }
    }
</script>

<template>
    <div class="panel__container">
        <SearchBarVue />
    </div>
</template>

<style>
    .panel__container{
        width: 40%;
        height: 100vh;
        background-color: rgba(0, 0, 0, 0.201);
        backdrop-filter: blur(5px);
        position: relative;
        padding-left: 3rem;
    }
</style>