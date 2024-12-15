<template>
<div class="body-card">
     <div class="card">
        <p>Дата и время:{{ new Date (weatherData.dt*1000)}}</p>
        <p>Температура:{{ weatherData.main.temp}}</p>
        <p>Погода:{{ weatherData.weather[0].main}}</p>
    <!-- <img :src="`https://openweathermap.org/img/wn/${weatherData.weather[0].icon}@2x.png`" alt=""> -->
        <div class="img-weather">
            <img :src="require(`@/assets/${idIconImage}`)" alt="" v-if="idIconImage">
        </div>
    </div>
</div>
   
</template>
<script>
export default{
    name:"ForecastCard",
    props:{
        weatherData:{
            type:Object
        },
        idIconImage:{
            type:String
        }
    }
}
</script>
<style scoped>
.card{
    border: 1px solid black;
    padding: 5px;
}


</style>