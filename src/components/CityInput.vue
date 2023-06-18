<template>
  <div class="mainComponent1">
    <h3>City name:</h3>
    <input
      v-model="cityName"
      type="text"
      placeholder="    city"
      class="inputStyle"
      @keyup.enter="getWeather()"
    />
    <button @click="getWeather" class="customButton">Get weather</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      cityName: "",
    };
  },
  methods: {
    getWeather() {
      this.$emit("get-weather", this.cityName);
    },
  },
};
</script>

<style scoped>
h3 {
  color: #fbfaf9;
}
.mainComponent1 {
  width: 180px;
  height: 140px;
  justify-content: center;
  display: flex;
  flex-wrap: wrap;
  align-items: center;
  margin: 0;
}
.inputStyle {
  border: none;
  width: 150px;
  height: 25px;
  padding-left: 10px;
  border-bottom: 2px solid #546978;
  background-color: #fbfaf9;
  border-radius: 20px;
  outline: none;
}
.customButton {
  border: 2px solid #fbfefd;
  border-radius: 25px;
  color: #fbfefd;
  padding: 5px;
  background-color: #546978;
}
.customButton:active {
  border: 2px solid pink;
  color: pink;
  transition: color 0.3s ease;
}
</style>
