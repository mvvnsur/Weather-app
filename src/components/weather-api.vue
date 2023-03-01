<template>
  <div class="main">
    <label class="select__city" for="city-select">Select a city:</label>
    <select id="city-select" v-model="selectedCity" @change="fetchWeather">
      <option v-for="city in cities" :key="city" :value="city">{{ city }}</option>
    </select>
    <div class="description" v-if="weather">
      <h2 class="selected">{{ selectedCity }}</h2>
      <p>{{ weather.weather[0].description }}</p>
      <p>{{ Math.round(weather.main.temp) }}°C</p>
      <img :src="'http://openweathermap.org/img/w/' + weather.weather[0].icon + '.png'" :alt="weather.weather[0].description">
    </div>
    <div v-else>
      <p>Loading...</p>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    apiKey: {
      type: String,
      required: true
    },
    defaultCity: {
      type: String,
      required: false,
      default: 'Grozny'
    }
  },
  data() {
    return {
      cities: ['Moscow', 'New-York', 'Tokyo', 'London','Grozny'],
      selectedCity: this.defaultCity,
      weather: null
    }
  },
  mounted() {
    this.fetchWeather()
  },
  methods: {
    fetchWeather() {
      const url = `https://api.openweathermap.org/data/2.5/weather?q=${this.selectedCity}&units=metric&appid=${this.apiKey}`
      fetch(url)
        .then(response => response.json())
        .then(data => {
          this.weather = data
        })
        .catch(error => {
          console.error(error)
        })
    }
  },
  watch: {
    selectedCity() {
      this.fetchWeather()
    }
  }
}
</script>

<style lang="scss" scoped>
.main{
  font-family: 'Roboto Mono', monospace;
  width: 500px;
  height: 500px;
  background-color: rgb(47, 64, 79);
  color: white;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  border-radius: 4px;
  margin: 50px auto;
}
.select__city{
  margin-bottom: 10px;
  font-size: 25px;
}

.selected{
  font-size: 95px;
  margin: 20px;
}

.description{
  text-align: center;
  font-size: 25px;
}

#city-select{
  padding: 6px 20px;
  font-family: 'Roboto Mono', monospace;
  font-size: 15px;
}
</style>