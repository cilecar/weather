<script setup>
import { ref } from "vue";
import axios from "axios";

const selectedCity = ref("");
const WeatherDiv = ref("none");
const weather = ref({
  main: {
    temp: null,
  },
  wind: {
    speed: null,
  },
  visibility: null,
  humidity: null,
  weatherDescription: null,
});
const error = ref(null);

const getWeather = () => {
  const apiKey = "2d34a40ca93596ea58608575b55f8a70";
  const url = `https://api.openweathermap.org/data/2.5/weather?q=${selectedCity.value}&appid=${apiKey}&units=metric`;

  axios.get(url).then((response) => {
    weather.value.main.temp = response.data.main.temp;
    weather.value.wind.speed = response.data.wind.speed;
    weather.value.visibility = response.data.visibility;
    weather.value.humidity = response.data.main.humidity;
    weather.value.weatherDescription = response.data.weather[0].description;
    error.value = null;
    WeatherDiv.value = "flex";
  });
};
</script>

<template>
  <div class=" min-w-fit w-1/2 m-auto flex flex-col mt-24 gap-2">
    <div class="flex w-full gap-2 bg-white p-4 rounded-lg">
      <img class="w-10 self-center" src="/src/img/pin-location.png" alt="" />
      <input class="outline-none text-black text-xl self-center w-full" placeholder="Введите название города" type="text"
        v-model="selectedCity" />
      <button @click="getWeather()">
        <img class="w-10" src="/src/img/search.png" alt="" />
      </button>
    </div>

    <div class="flex items-center flex-col gap-4 overflow-hidden bg-white p-4 rounded-lg" :style="{ display: WeatherDiv }">
      <div>
        <img class="w-1/3 m-auto" src="/src/img/weather.png" alt="" />
        <p class="w-max m-auto font-bold text-2xl" v-if="weather">
          {{ weather.main.temp }}°C
        </p>
        <p class="text-xl w-max m-auto" v-if="weather">
          {{ weather.weatherDescription }}
        </p>
      </div>
      <div class="flex justify-between w-full px-5 bg-gray-100 p-2 rounded-lg">
        <p class="flex gap-1" v-if="weather">
          <img class="w-6" src="/src/img/wind.png" alt="">
          {{ weather.wind.speed }} м/с
        </p>
        <p class="flex gap-1" v-if="weather">
          <img class="w-6" src="/src/img/eye.png" alt="">
          {{ weather.visibility }} м
        </p>
        <p class="flex gap-1" v-if="weather">
          <img class="w-6" src="/src/img/droplet.png" alt="">
          {{ weather.humidity }}%
        </p>
      </div>
    </div>
  </div>
</template>