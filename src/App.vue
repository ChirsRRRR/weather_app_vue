<template>
  <div id="app">
    <h1>Simple Weather App</h1>
    <form @submit.prevent="fetchWeather">
      <input type="text" v-model="city" placeholder="Enter a city name" />
      <button type="submit">Get Weather</button>
    </form>
    <div v-if="weather">
      <h2>{{ weather.name }}</h2>
      <p>{{ weather.main.temp }} °C</p>
      <p>{{ weather.weather[0].description }}</p>
    </div>
    <div v-if="error">{{ error }}</div>
  </div>
</template>

<script>
import axios from "axios";

const API_KEY = "187a4f2bb9b1fbd4d2c0329015cf8547";
const API_URL = "https://api.openweathermap.org/data/2.5/weather";

export default {
  data() {
    return {
      city: "",
      weather: null,
      error: null
    };
  },
  methods: {
    async fetchWeather() {
      try {
        const response = await axios.get(API_URL, {
          params: {
            q: this.city,
            appid: API_KEY,
            units: "metric"
          }
        });

        this.weather = response.data;
        this.error = null;
      } catch (error) {
        this.weather = null;
        this.error = "Failed to fetch weather data. Please check the city name and try again.";
      }
    }
  }
};
</script>

<style>
body {
  font-family: "Arial", sans-serif;
  background-color: #f0f0f0;
}

#app {
  max-width: 600px;
  margin: 0 auto;
  padding: 2rem;
  text-align: center;
  background-color: #fff;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
}

h1 {
  margin-bottom: 2rem;
}

input {
  width: 100%;
  padding: 0.5rem;
  margin-bottom: 1rem;
  font-size: 1rem;
  border: 1px solid #ccc;
  border-radius: 4px;
  box-sizing: border-box;
}

button {
  background-color: #007bff;
  color: #fff;
  border: none;
  padding: 0.5rem 1rem;
  font-size: 1rem;
  cursor: pointer;
  border-radius: 4px;
  transition: background-color 0.3s;
}

button:hover {
  background-color: #0056b3;
}

h2 {
  margin: 2rem 0 1rem;
}

p {
  margin: 0 0 1rem;
  font-size: 1.2rem;
}

.error {
  color: #e74c3c;
}
</style>
