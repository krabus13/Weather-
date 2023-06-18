<template>
  <div v-if="weatherData">
    <div class="firstElement">
      <h3>Today in {{ cityName }}:</h3>
      <h4>{{ weatherData.weather[0].description }}</h4>
      <h4>{{ formatTemperature(weatherData.weather[0].temperature) }} °C</h4>
      <img
        :src="getWeatherIconUrl(weatherData.weather[0].icon)"
        :alt="weatherData.weather[0].description"
      />
    </div>

    <h3>Weather today:</h3>
    <div class="element">
      <div
        v-for="hourlyWeather in weatherData.hourlyWeather"
        :key="hourlyWeather.time"
        class="form"
      >
        <div class="content">
          <p>{{ formatTemperature(hourlyWeather.temperature) }} °C</p>
          <h4>{{ hourlyWeather.description }}</h4>
          <p>{{ formatDateTime(hourlyWeather.time) }}</p>
        </div>
        <img
          :src="getWeatherIconUrl(hourlyWeather.icon)"
          :alt="hourlyWeather.description"
        />
      </div>
    </div>

    <h3>Weather for the next 5 days:</h3>
    <div class="element5days">
      <div
        v-for="forecast in weatherData.forecast"
        :key="forecast.date"
        class="form"
      >
        <div class="content2">
          <p>{{ formatDate(forecast.date) }}</p>
          <h4>{{ forecast.description }}</h4>
          <p>{{ formatTemperature(forecast.temperature) }} °C</p>
          <img
            :src="getWeatherIconUrl(forecast.icon)"
            :alt="forecast.description"
          />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    weatherData: Object,
    cityName: String,
  },
  methods: {
    formatTemperature(temperature) {
      return Math.round(temperature);
    },
    formatDate(date) {
      const options = { month: "short", day: "numeric" };
      return new Date(date).toLocaleDateString("en-US", options);
    },
    formatDateTime(dateTime) {
      const options = {
        month: "short",
        day: "numeric",
        hour: "numeric",
        minute: "numeric",
        hour12: false,
      };
      return new Date(dateTime).toLocaleDateString("en-US", options);
    },
    getWeatherIconUrl(icon) {
      return `http://openweathermap.org/img/w/${icon}.png`;
    },
  },
};
</script>
<style>
img {
  height: 60px;
  width: 60px;
}
h3 {
  padding: 15px;
}

.form {
  border-bottom: 3px solid #2a353c;
  width: 115px;
  height: 140px;
  font-size: 14px;
  margin: 5px;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column-reverse;
}
.element {
  display: flex;
  justify-content: center;
}
.element5days {
  display: flex;
  justify-content: center;
}
.firstElement {
  display: flex;
  justify-content: center;
  align-items: center;
}
.content {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  text-align: center;
}
.content2 {
  display: flex;
  flex-direction: column-reverse;
  justify-content: center;
  align-items: center;
  text-align: center;
}
.firstElement h4 {
  padding-left: 10px;
}

@media (max-width: 450px) {
  .element {
    flex-wrap: wrap;
  }
  .form {
    width: 70px;
    height: 160px;
  }
}
</style>
