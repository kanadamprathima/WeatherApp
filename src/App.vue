<template>
  <div class="container my-5">
    <h1 class="title text-center">Weather Forecast</h1>
    <form class="search-location" v-on:submit.prevent="getWeather">
      <input
        type="text"
        placeholder="Enter location"
        class="form-control text-muted form-rounded p-4 shadow-sm"
        v-model="citySearch"
        auto-complete="off"
      />
    </form>
    <div class="card rounded my-4 shadow-lg back-card overflow:hidden">
      <!-- top of card starts here -->
      <div class="card-top text-center" style="margin-bottom: 15rem">
        <div class="city-name my-3">
          <p>{{ Weather.cityName }}</p>
          <span>....</span>
          <p class="">{{ Weather.country }}</p>
        </div>
      </div>
      <!-- card middle body -->
      <div class="card-body">
        <div class="card-mid">
          <div class="row">
            <div class="col-12 text-center temp">
              <span>{{ Weather.temperature }}&deg;C</span>
              <p class="my-4">{{ Weather.description }}</p>
            </div>
          </div>
          <div class="row">
            <div class="col d-flex justify-content-between px-5 mx-5">
              <p>{{ Weather.highTemp }}&deg;C</p>
              <p>
                {{ Weather.lowTemp }}&deg;C
                <!-- <img src="./assets/logo.png" /> -->
              </p>
            </div>
          </div>
        </div>
        <!-- bottom card -->
        <div class="card-bottom px-5 py-4 row">
          <!-- starts here -->
          <div class="col text-center">
            <p>{{ Weather.feelsLike }}&deg;C</p>
            <span>feels like</span>
          </div>
          <div class="col text-center">
            <p>{{ Weather.humidity }}%</p>
            <span>humidity</span>
          </div>
        </div>
        <!-- card ends here -->
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      citySearch: "",
      Weather: {
        cityName: "Gwarinpa",
        country: "NG",
        temperature: 12,
        description: "Clouds everywhere",
        lowTemp: "19",
        highTemp: "30",
        feelsLike: "20",
        humidity: "55",
      },
    };
  },
  methods: {
    async getWeather() {
      console.log(this.citySearch);
      const key = "2cfe5922ed02db8c9175de7178fb0bf8";
      const baseURL = `https://api.openweathermap.org/data/2.5/weather?q=${this.citySearch}&appid=${key}`;
      const response = await fetch(baseURL);
      const data = await response.json();
      console.log(data);
      this.citySearch = "";
    },
  },
};
</script>

<style>
@import "./assets/styles.css";
</style>
