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
          return "»" + this.city + "«";
        },
        temp() {
          return "Teplota: " + Math.floor(this.info.main.temp) + "°C";
        },
        feelsLike() {
          return "Pocitová teplota: " + Math.floor(this.info.main.feels_like) + "°C";
        },
        wind() {
          return "Rychlost větru: " + this.info.wind.speed + " Km";
        }

      },
    methods: {
      getWheather() {
        if (this.city.trim().length < 2) {
          this.error = "Název města musí obsahovat alespoň dva znaky";
          return;
        } this.error = "";
          axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=7895800c9e6dc3b898de4b3a0b22028b`)
              .then(res => (this.info = res.data))
      }
    }
    }
  </script>

<template>
<div class="wrapper">
  <h1>Počasí ve vašem městě</h1>
  <p>{{ city == ""? "Zjistit počasí ve vašem městě" : "Zjistit počasí v " + cityName}}</p>
  <input type="text" v-model="city" placeholder="Zadejte město" @keydown.enter="getWheather()">
  <button @click="getWheather()" v-if="city !== ''">Zjistit počasí</button>
  <button class="transparent-button" disabled v-else>Zjistit počasí</button>
  <p class="error">{{ error }}</p>
  <div v-if="info !== null">
    <p class="teplota">{{ temp }}</p>
    <p>{{ feelsLike }}</p>
    <p>{{ wind }}</p>
  </div>
</div>
</template>

<style scoped>
.wrapper {
width: 900px;
height: 500px;
border-radius: 50px;
background: #1f0f24;
padding: 20px;
text-align: center;
color: white;
}

.wrapper h1 {
  margin-top: 50px;
}

.wrapper p {
  margin-top: 20px;
}

.wrapper input {
  margin-top: 30px;
  background: transparent;
  border: 0;
  border-bottom: 2px solid #110813;
  color: #fcfcfc;
  font-size: 14px;
  padding: 5px 8px;
  outline: none;
}

.wrapper input:focus {
  border-bottom-color: #6e2d7d;
}

.wrapper button {
  background-color: #e3bc4b;
  color: #fff;
  border-radius: 10px;
  border: 2px solid #b99935;
  padding: 10px 15px;
  margin-left: 20px;
  cursor: pointer;
  transition: transform 500ms ease;
}

.wrapper .transparent-button:hover {
  transform: scale(1);
}
.wrapper button:hover {
  transform: scale(1.05);
}

.wrapper .transparent-button {
  background: transparent;
  backdrop-filter: blur(2px);
  cursor: not-allowed;
}

.error {
  color: #d03939;
}

.teplota {
  font-size: 40px;
}
</style>