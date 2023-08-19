<script setup>
import { ref } from 'vue';
import axios from 'axios';

const selectedCity = ref('');
const weather = ref(null);
const error = ref(null);

const getWeather = () => {
  const apiKey = '2d34a40ca93596ea58608575b55f8a70';
  const url = `https://api.openweathermap.org/data/2.5/weather?q=${selectedCity.value}&appid=${apiKey}&units=metric`;

  axios.get(url)
    .then(response => {
      weather.value = response.data;
      error.value = null;
    })
};
</script>

<template>
  <div class="bg-zinc-900">
      <input type="text" v-model="selectedCity">
      <button @click="getWeather()">Получить погоду</button>
      <p v-if="weather">{{ weather.main.temp }}°C</p>
  </div>
</template>