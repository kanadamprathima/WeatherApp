<template>
  <div id="main" :class="isDay ? 'day' : 'night'">
    <div class="container my-4">
      <h1 class="title text-center">Weather Forecast</h1>
      <form class="search-location" v-on:submit.prevent="getWeather">
        <input
          type="text"
          placeholder="Enter location"
          class="form-control text-muted p-4 form-rounded shadow-sm"
          v-model="citySearch"
          auto-complete="off"
        />
      </form>
      <p class="text-center my-3" v-if="cityFound">No city found</p>

      <div
        class="card rounded my-3 shadow-lg back-card overflow-hidden"
        v-if="visible"
      >
        <div>
          <div icon="sunny" v-if="clearSky">
            <span class="sun"></span>
          </div>
          <div icon="snowy" v-if="snowy" data-label="Chilly">
            <ul>
              <li></li>
              <li></li>
              <li></li>
              <li></li>
              <li></li>
              <li></li>
              <li></li>
              <li></li>
              <li></li>
              <li></li>
              <li></li>
              <li></li>
              <li></li>
            </ul>
          </div>
          <div icon="stormy" v-if="stormy" data-label="Soggy">
            <span class="cloud"></span>
            <ul>
              <li></li>
              <li></li>
              <li></li>
              <li></li>
              <li></li>
            </ul>
          </div>
          <div icon="cloudy" v-if="cloudy" data-label="Perfect">
            <span class="cloud"></span>
            <span class="cloud"></span>
          </div>
          <div icon="nightmoon" v-if="clearNight" data-label="Cool!">
            <span class="moon"></span>
            <span class="meteor"></span>
          </div>
        </div>

        <!-- top of card starts here -->
        <div class="card-top text-center" style="margin-bottom: 10rem">
          <div class="city-name my-3">
            <p>{{ weather.cityName }}</p>
            <span>....</span>
            <p class="">{{ weather.country }}</p>
          </div>
        </div>
        <!-- card middle body -->
        <div class="card-body">
          <div class="card-mid">
            <div class="row">
              <div class="col-12 text-center temp">
                <h1 class="title text-center">Todays weather</h1>
                <span>{{ weather.temperature }}&deg;C</span>
                <p class="my-4">{{ weather.description }}</p>
              </div>
            </div>
            <div class="row">
              <div class="col d-flex justify-content-between px-5 mx-5">
                <p>{{ weather.highTemp }}&deg;C</p>
                <p>
                  {{ weather.lowTemp }}&deg;C
                  <!-- <img src="./assets/logo.png" /> -->
                </p>
              </div>
            </div>
          </div>
          <!-- bottom card -->
          <div class="card-bottom px-5 py-4 row">
            <!-- starts here -->
            <div class="col text-center">
              <p>{{ weather.feelsLike }}&deg;C</p>
              <span>feels like</span>
            </div>
            <div class="col text-center">
              <p>{{ weather.humidity }}%</p>
              <span>humidity</span>
            </div>
          </div>
          <!-- card ends here -->
        </div>
        <!-- tomorrow weather -->
        <div class="card-body text-center temp">
          <h1 class="title text-center">Tomorrow weather</h1>
          <span>{{ tomorrowWeather.temperature }}&deg;C</span>
          <p class="my-4">{{ tomorrowWeather.description }}</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "App",
  data() {
    return {
      cityFound: false,
      visible: true,

      stormy: false,
      cloudy: false,
      clearSky: false,
      clearNight: true,
      snowy: false,
      isDay: true,
      citySearch: "",
      tomorrowWeather: {
        temperature: 12,
        description: "windy",
      },
      weather: {
        cityName: "Amsterdam",
        country: "NL",
        temperature: 12,
        description: "Clouds everywhere",
        lowTemp: 19,
        highTemp: 30,
        feelsLike: 20,
        humidity: 55,
      },
    };
  },
  methods: {
    async getWeather() {
      console.log(this.citySearch);
      try {
        const key = "2cfe5922ed02db8c9175de7178fb0bf8";
        //for details of weather
        const baseURL = `https://api.openweathermap.org/data/2.5/weather?q=${this.citySearch}&appid=${key}&units=metric`;
        //daily weatherforecast
        const response1 = await axios.get(
          `https://api.openweathermap.org/data/2.5/forecast?q=${this.citySearch}&appid=${key}&units=metric`
        );
        const response = await fetch(baseURL);
        const data = await response.json();
        console.log(data);
        console.log("tomorrows weather response", response1.data);
        // tomorrow weather details
        this.tomorrowWeather.description =
          response1.data.list[7].weather[0].description;
        this.tomorrowWeather.temperature = Math.round(
          response1.data.list[7].main.temp
        );
        //All the weather details
        this.citySearch = "";
        this.weather.cityName = data.name;
        this.weather.country = data.sys.country;
        this.weather.temperature = Math.round(data.main.temp);
        this.weather.description = data.weather.map((m) => m.description);
        this.weather.lowTemp = Math.round(data.main.temp_min);
        this.weather.highTemp = Math.round(data.main.temp_max);
        this.weather.feelsLike = Math.round(data.main.feels_like);
        this.weather.humidity = Math.round(data.main.humidity);
        const timeOfDay = data.weather[0].icon;
        //check for day or night & add :class:day /night in main div component
        if (timeOfDay.includes("n")) {
          this.isDay = false;
        } else {
          this.isDay = true;
        }
        const mainWeather = data.weather[0].main;
        if (mainWeather.includes("Clouds")) {
          this.stormy = false;
          this.cloudy = true;
          this.clearNight = false;
          this.clearSky = false;
          this.snowy = false;
        }
        if (mainWeather.includes("Thunderstorm")) {
          this.stormy = true;
          this.cloudy = false;
          this.clearNight = false;
          this.clearSky = false;
          this.snowy = false;
        }
        if (mainWeather.includes("Clouds")) {
          this.stormy = false;
          this.cloudy = true;
          this.clearNight = false;
          this.clearSky = false;
          this.snowy = false;
        }
        if (mainWeather.includes("Clear") && this.isDay) {
          this.stormy = false;
          this.cloudy = false;
          this.clearNight = false;
          this.clearSky = true;
          this.snowy = false;
        }
        if (mainWeather.includes("Clouds") && !this.isDay) {
          this.stormy = false;
          this.cloudy = false;
          this.clearNight = true;
          this.clearSky = false;
          this.snowy = false;
        }
        if (mainWeather.includes("Snow")) {
          this.stormy = false;
          this.cloudy = false;
          this.clearNight = false;
          this.clearSky = false;
          this.snowy = true;
        }

        this.visible = true;
      } catch (e) {
        console.log(e.message);
      }
    },
  },
};
</script>

<style>
@import "./assets/styles.css";
@import "./assets/animation.css";
</style>
