<template>
  <input v-model="query" @keyup.enter="search()" type="text" placeholder="Введите город, чтобы узнать погоду"
         class="input input-bordered w-full max-w-xl bg-input"/>
  <div class="flex flex-col flex-1 items-center" v-if="weatherData">
    <weather-current
        :city="city"
        :weatherData="weatherData"
        :weatherImageUrl="weatherImageUrl"
    />

    <hr class="border-white border-opacity-10 border w-full"/>

    <weather-hourly
        :city="city"
        :weatherData="weatherData"
        :weatherImageUrl="weatherImageUrl"
    />

    <hr class="border-white border-opacity-10 border w-full"/>


    <weather-weekly
        :city="city"
        :weatherData="weatherData"
        :weatherImageUrl="weatherImageUrl"
    />

  </div>
</template>

<script>
import axios from "axios";
import * as config from '../config.js'
import WeatherCurrent from "./components/WeatherCurrent.vue";
import WeatherHourly from "./components/WeatherHourly.vue";
import WeatherWeekly from "./components/WeatherWeekly.vue";

export default {
  name: "App",
  components: {WeatherWeekly, WeatherHourly, WeatherCurrent},
  data() {
    return {
      weatherImageUrl: config.IMAGE_URL,
      weatherData: null,
      query: null,
      city: null,
      geolocation: null
    }
  },
  methods: {
    async getWeatherData() {
      if (this.city.geometry.coordinates[1] && this.city.geometry.coordinates[0]) {
        const {data} = await axios.get(config.WEATHER_URL, {
          params: {
            'lat': this.city.geometry.coordinates[1],
            'lon': this.city.geometry.coordinates[0],
            'exclude': '{part}',
            'appid': config.API_KEY,
            'lang': 'ru',
            'units': 'metric'
          }
        })
        this.weatherData = data
      }
    },
    async getCity() {
      const {data} = await axios.get(`${config.CITY_URL}${this.query}.json`, {
        params: {
          'types': 'place',
          'language': 'ru',
          'access_token': config.MAPBOX_API_KEY
        }
      });
      this.city = data.features[0]
    },
    async search() {
      await this.getCity()
      await this.getWeatherData()
    },
    async getGeolocation() {
      navigator.geolocation.getCurrentPosition(this.handleGeolocation);
    },
    async getWeatherDataByGeolocation() {
      if (this.geolocation) {
        const {data} = await axios.get(config.WEATHER_URL, {
          params: {
            'lat': this.geolocation.lat,
            'lon': this.geolocation.lon,
            'exclude': '{part}',
            'appid': config.API_KEY,
            'lang': 'ru',
            'units': 'metric'
          }
        })
        this.weatherData = data
      }
    },
    handleGeolocation(position) {
      this.geolocation = {
        'lat': position.coords.latitude,
        'lon': position.coords.longitude
      }
      this.getWeatherDataByGeolocation()
    }
  },
  async created() {
    await this.getGeolocation()
  }
}
</script>
<style>
.bg-input {
  background-color: #242424;
}
</style>
