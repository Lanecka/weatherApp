<script>
import axios from 'axios';

export default {
  data() {
    return {
      city: "",
      error: "",
      info: null
    }
  },
  computed: {
    cityName() {
      return "«" + this.city + "»"
    },
    showTemp() {
      return "Температура: " + this.info.main.temp
    },
    showMinTemp() {
      return "Минимальная температура: " + this.info.main.temp_min
    },
    showMaxTemp() {
      return "Максимальная температура: " + this.info.main.temp_max
    }
  },
  methods: {
    getWeather() {
      if(this.city.trim().length < 2) {
        this.error = "Введите название больше одного символа"
        return false
      }

      this.error = ""

      axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=3b6c2024cd5f5c26031c65605da22645`)
        .then(res => (this.info = res.data))
    }
  }
};
</script>

<template>
  <div class="wrapper">
    <h1>Приложение погоды</h1>
    <p>Узнайте погоду в {{ city == "" ? "вашем городе" : cityName }}</p>
    <input type="text" v-model="city" placeholder="Введите город" />
    <button v-if="city != ''" @click="getWeather()">Узнать</button>
    <button disabled v-else>Узнать</button>
    <p class="error">{{ error }}</p>

    <div v-if="info != null">
      <p>{{ showTemp }} </p>
      <p>{{ showMinTemp }} </p>
      <p>{{ showMaxTemp }} </p>
    </div>
  </div>
</template>

<style scoped>
.wrapper {
  width: 1000px;
  height: 550px;
  border-radius: 10px;
  background: rgb(35, 39, 42);
  padding: 50px 20px 20px;
  text-align: center;
  color: aliceblue;
}

.wrapper h1 {
  margin-bottom: 20px;
}

.wrapper p {
  margin-bottom: 40px;
}

.wrapper input {
  background: transparent;
  border: 0;
  border-bottom: 2px solid rgb(27, 30, 33);
  color: azure;
  font-size: 14px;
  padding: 5px 8px;
  outline: none;
  margin-right: 20px;
}

.wrapper input:focus {
  border-bottom-color: rgb(153, 0, 255);
}

.wrapper button {
  background: rgb(0, 162, 255);
  color: aliceblue;
  border-radius: 5px;
  padding: 12px 20px;
  cursor: pointer;
  transition: transform 500ms ease;
}

.wrapper button:disabled {
  background: rgb(48, 56, 60);
  color: rgba(240, 248, 255, 0.395);
  cursor: not-allowed;
}

.wrapper button:hover {
  transform: scale(1.1) translateY(-5px);
}

.error {
  color: red;
}
</style>
