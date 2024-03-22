<script>
import axios from 'axios'

export default {
  data(){
    return{
      city: '',
      error: '',
      info: null,
    }
  },
  computed: {
    cityName(){
      return '«' + this.city + '»'
    },
    showTemp() {
      return 'Температура: ' + this.info.current.temp_c
    },
    showFeelsLike() {
      return 'Ощущается как: ' + this.info.current.feelslike_c
    },
    showWind() {
      return 'Ветер: ' + this.info.current.wind_mph + 'км/ч'
    },
    showPrecip() {
      return 'Осадки: ' + this.info.current.precip_mm + 'мм'
    },
  },
  methods:{
    getWeather() {
      if(this.city.trim().length < 3) {
        this.error = 'Нужно название более одного символа :>'
        return fals
      }

      this.error = ''

      axios.get(`https://api.weatherapi.com/v1/current.json?key=%200c2073409b6c4279bae94510242103&q=${this.city}&aqi=no`)
        .then(res =>(this.info = res.data))
    }
  }
}
</script>

<template>
  <div className="wrapper">
    <h1>Погодное приложение</h1>
    <p>Узнать погоду в {{ city == ''? 'вашем городе' : cityName}}</p>
    <input type="text" v-model="city" placeholder="введите город (en)">
    <button v-if="city != ''" @click="getWeather()">Получить погоду</button>
    <button disabled v-else>Введите название города</button>
    <p className="error"> {{ error }}</p>

    <div v-if="info != null" className="weatherRep">
      <div className="weatherEl">
        <p>{{ showTemp }}</p>
        <p>{{ showFeelsLike }}</p>
      </div>
      <div className="weatherEl">
        <p>{{ showWind }}</p>
        <p>{{ showPrecip }}</p>
      </div>
    </div>
  </div>
</template>

<style scoped>
.error{
  color: red;
}

.wrapper{
  width: 900px;
  height: 500px;
  border-radius: 50px;
  padding: 20px;
  background: #ffffff;
  color: black;
  text-align: center;
}

.wrapper h1{
  margin-top: 50px;
}

.wrapper p{
  margin-top: 20px;
}

.wrapper input{
  margin-top: 30px;
  background: transparent;
  border: 0;
  border-bottom: 2px solid black;
  color: #3d3d3d;
  font-size: 14px;
  padding: 5px 8px;
  outline: none;
}

.wrapper input:focus{
  border-bottom-color: #30c16a;
}

.wrapper button:disabled{
  background: #1e2285;
  border-color: #5a60ff;
  cursor: not-allowed;
}

.wrapper button{
  background: #2b30c0;
  color: rgb(255, 255, 255);
  border-radius: 10px;
  border: 2px solid rgb(57, 105, 195);
  padding: 10px 15px;
  margin-left: 20px;
  cursor: pointer;
  transition: transform 500ms ease;
}

.wrapper button:hover{
  border-color: #38d777;
  background: #30c16a;
  transform: scale(1.1) translateY(-5px);
}

.wrapper button:hover:disabled{
  background: #1e2285;
  border-color: #5a60ff;
}

.weatherRep{
  display: flex;
  flex-direction: column;
}

.weatherEl{
  display: flex;
  justify-content: center;
  margin: 15px;
  gap: 25px;
}
</style>
