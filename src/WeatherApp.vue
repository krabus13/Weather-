<template>
  <div class="start">
    <city-input @get-weather="getWeather"></city-input>
    <div v-if="weatherData" :class="['main', { show: showMain }]">
      <weather-display
        :weather-data="weatherData"
        :city-name="cityName"
      ></weather-display>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import { BASE_URL, API_KEY } from "@/constants";
import CityInput from "./components/CityInput.vue";
import WeatherDisplay from "./components/WeatherDisplay.vue";

export default {
  components: {
    CityInput,
    WeatherDisplay,
  },
  data() {
    return {
      cityName: "",
      weatherData: null,
      weatherApiKey: API_KEY,
      imageApiKey: "",
      showMain: false,
    };
  },
  methods: {
    async getWeather(cityName) {
      try {
        cityName =
          cityName.charAt(0).toUpperCase() + cityName.slice(1).toLowerCase();

        const weatherResponse = await axios.get(
          `${BASE_URL}?q=${cityName}&appid=${this.weatherApiKey}&units=metric`
        );

        const forecastResponse = await axios.get(
          `https://api.openweathermap.org/data/2.5/forecast?q=${cityName}&appid=${this.weatherApiKey}&units=metric`
        );

        const hourlyWeatherResponse = await axios.get(
          `https://api.openweathermap.org/data/2.5/forecast?q=${cityName}&appid=${this.weatherApiKey}&units=metric&cnt=8`
        );

        const weatherData = {
          weather: [
            {
              description: weatherResponse.data.weather[0].description,
              temperature: weatherResponse.data.main.temp,
              icon: weatherResponse.data.weather[0].icon,
            },
          ],
          forecast: [],
          hourlyWeather: [],
        };

        for (let i = 1; i < forecastResponse.data.list.length; i += 8) {
          const forecast = forecastResponse.data.list[i];
          const forecastData = {
            date: forecast.dt_txt,
            description: forecast.weather[0].description,
            temperature: forecast.main.temp,
            icon: forecast.weather[0].icon,
          };
          weatherData.forecast.push(forecastData);
        }

        for (let i = 0; i < hourlyWeatherResponse.data.list.length; i++) {
          const hourlyWeather = hourlyWeatherResponse.data.list[i];
          const hourlyWeatherData = {
            time: hourlyWeather.dt_txt,
            description: hourlyWeather.weather[0].description,
            temperature: hourlyWeather.main.temp,
            icon: hourlyWeather.weather[0].icon,
          };
          weatherData.hourlyWeather.push(hourlyWeatherData);
        }

        this.weatherData = weatherData;
        this.cityName = cityName;

        setTimeout(() => {
          this.showMain = true;
        }, 100);
      } catch (error) {
        console.error(error);
      }
    },
  },
};
</script>

<style>
* {
  padding: 0;
  margin: 0;
  box-sizing: border-box;
  font-family: "Franklin Gothic Medium", sans-serif;
}
body {
  background-color: #546978;
  color: #0c0c03;
  opacity: 0.7;
}
.main {
  background-color: #fbfaf9;
  border-radius: 15px;
  padding: 10px;
  width: auto;
  height: auto;
  margin: 20px;
  opacity: 0;
  transition: opacity 0.5s ease;
}
.main.show {
  opacity: 1;
}
.start {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-wrap: wrap;
  flex-direction: column;
  margin-top: 40px;
}
@media (max-width: 450px) {
  .main {
    width: 95%;
    height: 700px;
    padding: 0;
    margin: 0;
  }
}
</style>
