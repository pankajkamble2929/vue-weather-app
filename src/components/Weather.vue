<template>
  <div class="container mt-5">
    <h1 class="text-center mb-5">WEATHER APP</h1>
    <div class="input-group mb-3">
      <input type="text" class="form-control" placeholder="Enter city name" v-model="city" @keyup.enter="getWeather">
      <button class="btn btn-primary" type="button" @click="getWeather">Get Weather</button>
    </div>
    <div v-if="loading" class="text-center">
      <div class="spinner-border text-primary" role="status">
        <span class="visually-hidden">Loading...</span>
      </div>
    </div>
    <div v-if="error" class="alert alert-danger mt-3">{{ error }}</div>
    <div v-if="weather" class="mt-5 px-5">
      <h2>{{ weather.name }}</h2>
      <div class="row">
        <div class="col-md-4">
          <p class="mb-0">Temperature:</p>
          <p class="lead">{{ weather.main.temp }}°C</p>
          <p class="mb-0">Feels Like:</p>
          <p class="lead">{{ weather.main.feels_like }}°C</p>
          <p class="mb-0">Humidity:</p>
          <p class="lead">{{ weather.main.humidity }}%</p>
        </div>
        <div class="col-md-4 text-center">
          <p class="mb-0">Weather:</p>
          <p class="lead">{{ weather.weather[0].description }}</p>
          <img :src="'http://openweathermap.org/img/w/' + weather.weather[0].icon + '.png'" :alt="weather.weather[0].description">
        </div>
        <div class="col-md-4">
          <p class="mb-0">Wind Speed:</p>
          <p class="lead">{{ weather.wind.speed }}m/s</p>
          <p class="mb-0">Wind Direction:</p>
          <p class="lead">{{ weather.wind.deg }}°</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'Weather',
  data() {
    return {
      city: '',
      loading: false,
      error: '',
      weather: null,
    };
  },
  methods: {
    async getWeather() {
      try {
        this.loading = true;
        const response = await axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=de563453052e0125fc061291e0dc8fce`);
        this.loading = false;
        this.weather = response.data;
      } catch (error) {
        this.loading = false;
        if (error.response && error.response.status === 404) {
          this.error = 'City not found';
        } else {
          this.error = 'Please Enter City Name';
        }
      }
    },
  },
};
</script>

<style>

</style>
