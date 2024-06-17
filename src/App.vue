<script>
  import axios from 'axios';
  
  export default {
    data() {
      return {
        city: "",
        error: "",
        info: null,
        weatherApiKey: import.meta.env.VITE_WEATHER_API
      }
    },
    computed: {
      cityName() {
        return `"` + this.city + `"`
      }
    },
    methods: {
      getWeather() {
        if(this.city.trim().length < 2) {
          this.error = `Нужно корректное название города!`
          return false
        }
        this.error = ""

        axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=${this.weatherApiKey}`)
          .then(res => (this.info = res.data))
      },
      showTemp() {
        return `Температура: ${this.info.main.temp}`
      },
      showFeelsLike() {
        return `Ощущается как: ${this.info.main.feels_like}`
      },
      showMinTemp() {
        return `Минимальная температура: ${this.info.main.temp_min}`
      },
      showMaxTemp() {
        return `Максимальная температура: ${this.info.main.temp_max}`
      }
    }
  }
</script>


<template>
  <div class="wrapper">
    <h1>Погодное приложение</h1>
    <p>Узнать погоду в {{ city === "" ? `вашем городе` : cityName }}</p>

    <input type="text" placeholder="Введите город" v-model="city">
    <button v-if="city != ``" @click="getWeather()">Получить погоду</button>
    <button disabled v-else>Введите город</button>
    <p class="error">{{ error }}</p>
    <p v-if="info != null">{{ info.main.temp }}</p>
    
  </div>
</template>


<style scoped>

  .wrapper {
    width: 900px;
    height: 500px;
    border-radius: 50px;
    background: #1f0f24;
    text-align: center;
    color: #f1f1f1;
  }

  .wrapper h1 {
    padding-top: 50px;
  }

  .wrapper p {
    margin-top: 20px;
  }

  .wrapper input{
    margin-top: 30px;
    background: transparent;
    border: 0;
    border-bottom: 2px solid #110813;
    color: #fcfcfc;
    font-size: 14px;
    padding: 5px 8px;
    outline: none;
  }

  .wrapper input:focus {
    border-bottom-color: #6e2d7d;
  }

  .wrapper button {
    background: #e3bc4b;
    color: #f1f1f1;
    border-radius: 10px;
    border: 2px solid #b99935;
    padding: 10px 15px;
    margin-left: 20px;
    cursor: pointer;
    transition: transform 500ms ease;
  }
  .wrapper button:disabled{
    background: #5d4423;
    cursor: not-allowed;
  }
  .wrapper button:hover {
    transform: scale(1.1) translateY(-5px);
  }

  .error {
    color: #d03939;
  }
</style>
