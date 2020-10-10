<template>
  <div class="container py-2 text-center">
    <div class="search-bar mt-3">
      <input
        @keyup.enter="getWeatherBySearch"
        v-model="search"
        type="text"
        class="form-control bg-transparent"
        placeholder="Search your location"
      />
    </div>

    <div class="weather-results d-flex flex-column" v-if="weatherData">
      <div class="location-info">
        <h1 class="location">{{ weatherData.name }}</h1>
      </div>
      <div class="clouds-status">
        <p>{{ weatherData.weather[0].main }}</p>
      </div>
      <div class="temperature-info d-flex justify-content-center">
        <h2>{{ Math.round(weatherData.main.temp) }}</h2>
        <p>&deg;c</p>
      </div>
      <div class="weather-status">
        <img
          :src="`http://openweathermap.org/img/wn/${weatherData.weather[0].icon}@2x.png`"
          alt=""
        />
      </div>
    </div>
    
    <div class="weather-else text-center" v-else>
      <h1>iCreative<br />Weather</h1>
    </div>

    <div class="button-find">
      <button class="btn btn-info btn-block btn-lg" @click="getLocation">
        Find my location
      </button>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "App",
  data() {
    return {
      search: "",
      lat: null,
      weatherData: null,
      lon: null,
      apiUrl: "https://api.openweathermap.org/data/2.5/weather",
      apiKey: "d7b0ca7973f04988f122ef97ed610b8d",
    };
  },
  methods: {
    getLocation() {
      navigator.geolocation.getCurrentPosition((position) => {
        this.lat = position.coords.latitude;
        this.lon = position.coords.longitude;
        this.getWeatherByCoords();
      });
    },
    getWeatherByCoords() {
      axios(
        `${this.apiUrl}?lat=${this.lat}&lon=${this.lon}&appid=${this.apiKey}&units=metric`
      ).then((response) => {
        console.log(response);
        this.weatherData = response.data;
      });
    },
    getWeatherBySearch() {
      axios(
        `${this.apiUrl}?q=${this.search}&appid=${this.apiKey}&units=metric`
      ).then((response) => {
        this.weatherData = response.data;
      });
    },
  },
};
</script>

<style>
#app {
  min-height: 100vh;
  height: 100%;
  color: white;
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
  overflow: hidden;
  background: url("../src/assets/images/bg-night.jpg");
}
.weather-results {
  margin-top: 18vh;
}
.button-find {
  position: absolute;
  bottom: 0;
  right: 0;
  width: 100%;
}
input[type="text"] {
  border: 2px solid #aaaaaa;
  border-radius: 8px;
  color: white !important;
}

input::placeholder {
  color: rgb(180, 177, 177) !important;
}

.weather-else{
  margin-top: 30vh;
  font-weight: 200;
}
</style>
