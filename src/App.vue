<template>
  <div class="wrapper">
    <h1>Weather App</h1>
    <p>Search city {{ city == "" ? "weather" : cityName }}</p>
    <div class="btn-inpt-wrap">
      <input type="text" v-model="city" placeholder="write City" />
      <button
        class="bg-[#1da1f2] text-white"
        v-if="city != ''"
        @click="getWeather()"
      >
        Get weather
      </button>
      <button disabled v-else>Write name of city</button>
    </div>
    <div v-if="info != null" class="temp-text">
      <a class="error">{{ error }}</a>
      <a class="info-temp">{{ showTemp }} °C</a>
      <a class="info-temp">{{ showFeelsLike }} °C</a>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      city: "",
      error: "",
      info: null,
    };
  },

  computed: {
    cityName() {
      return "'" + this.city + "'";
    },
    showTemp() {
      return "temerature is: " + this.info.main.temp + "";
    },
    showFeelsLike() {
      return "feels like : " + this.info.main.feels_like + "";
    },
  },

  methods: {
    getWeather() {
      if (this.city.trim().length < 2) {
        this.error = "write all name of the city";
        return false;
      }

      this.error = "";

      axios
        .get(
          `https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=1456f71ad4e7bd0544ca93ec82930b67`
        )
        .then((res) => (this.info = res.data));
    },
  },
};
</script>

<style lang="css" scoped>
@import "./assets/tailwind.css";

.wrapper {
  width: 700px;
  height: 300px;
  border-radius: 50px;
  padding: 20px;
  background: rgb(230 226 255);
  color: rgb(44, 44, 44);
  font-family: "Segoe UI", Tahoma, Geneva, Verdana, sans-serif;
}
.wrapper h1 {
  margin-top: 20px;
  text-align: center;
  color: rgb(44, 44, 44);
}
.wrapper p {
  text-align: center;
  margin-top: 15px;
  color: rgb(44, 44, 44);
}

.wrapper input {
  background: transparent;
  border: 0;
  border-bottom: 2px solid rgb(44, 44, 44);
  font-size: 14px;
  padding: 5px 8px;
  outline: none;
}
.wrapper input:focus {
  border-bottom-color: #7e22ce;
}

.wrapper button:disabled {
  background: rgb(162, 162, 162);
  cursor: not-allowed;
}
.wrapper button:disabled:hover {
  background: rgb(176, 134, 98);
  cursor: not-allowed;
}
.wrapper button {
  border-radius: 10px;
  border: 1px solid white;
  padding: 10px 15px;
  margin-left: 20px;
  cursor: pointer;
  transition: transform 150ms ease-in-out;
}
.wrapper button:hover {
  transform: scale(1.07) translateY(-3px);
  border: 1px solid rgb(74, 49, 107);
  background: #794dc3;
  transition: transform 150ms ease-in-out;
}
.wrapper button:active {
  background-color: rgb(99, 51, 166);
  transition: transform 150ms ease-in-out;
}
.btn-inpt-wrap {
  align-items: center;
  justify-content: center;
  display: flex;
  margin-top: 30px;
}

.error {
  color: rgb(154, 154, 154);
  align-items: center;
  justify-content: center;
  display: flex;
}
.info-temp {
  align-items: center;
  justify-content: center;
  display: flex;
  font-size: 30px;
  font-weight: bold;
  margin-top: 20px;
}
</style>
