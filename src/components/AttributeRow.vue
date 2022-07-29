<script>
    export default{
        props:{
            title: String,
            value: String
        },
        data(){
            return{
                
            }
        }
    }
</script>

<template>
    <div>
        <p id="title">{{ title }}</p>
        <p id="value"> {{ value }}</p>
    </div>
</template>

<style scoped>
    div{
        display: flex;
        flex-direction: row;
        justify-content: space-between;
        width:90%;
    }

    #title {
        color: rgb(204, 204, 204);
    }

    #value {
        color: rgb(255, 255, 255);
    }
</style>