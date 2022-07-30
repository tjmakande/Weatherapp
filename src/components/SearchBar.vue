<script>
    export default {
        methods:{
            SubmitClicked() {
                this.$emit('newLocation', this.SearchedLocation);
            }
        },
        props:{
            item: Function
        },
        mounted(){
            document
                .querySelector('.search__location')
                .addEventListener('keyup', e => {
                    if(e.key === "Enter"){
                        this.$emit('newLocation', this.SearchedLocation)
                    }
                })
        },
        data(){
            return{
                SearchedLocation: ''
            }
        }
    }
</script>

<template>
    <div class="searchbar__container">
        <input class="search__location" v-model="SearchedLocation" type="text" placeholder="Search location..."/>
        <div @click="SubmitClicked"> Submit </div>
    </div>
</template>

<style scoped>
    .searchbar__container{
        display: flex;
        justify-content: space-between;
        position: relative;
    }
    .searchbar__container input{
        width: 70%;
        height: 5rem;
        background: transparent;
        border:0;
        border-bottom: 1px solid gray;
        color: white;
        font-size: 1.6rem;
    }

    .searchbar__container input:focus{
        outline: none;
    }

    .searchbar__container div{
        height: 5rem;
        padding: 1px 2px;
        margin: 0;
        width: 5rem;
        display: flex;
        align-items: center;
        justify-content: center;
        background-color: red;
        position: absolute;
        right:-3rem;
    }
</style>