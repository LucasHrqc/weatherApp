<script>


export default {
  name: 'WeatherAppComponent',
  components: {},
  props: [],
  data() {
    return {
        location: '',
        weatherImages: '',
        temperature: '',
        description: '',
        windSpeed: '',
        humidity: '',
    }
  },
  methods: {
    callApi() {
      const container = document.querySelector('.container');
      const weatherBox = document.querySelector('.weather-box');
      const weatherDetails = document.querySelector('.weather-details');
      const error404 = document.querySelector('.not-found');



      const apiKey = '5870d1fb63dda1672ccec99487744069';
      const api = `https://api.openweathermap.org/data/2.5/weather?q=${this.location}&units=metric&appid=${apiKey}`;

      if (this.location === '') return;

      fetch(api).then((response) => response.json()).then(json => {
        
        if (json.cod === '404') {
          container.style.height = '400px';
          weatherBox.style.display = 'none';
          weatherDetails.style.display = 'none';
          error404.style.display = 'block';
          error404.classList.add('fadeIn');
          return;
        }

        error404.style.display = 'none';
        error404.classList.remove('fadeIn');

        console.log(json);
        this.weatherImages = '/public/' + json.weather[0].main.toLowerCase().replace(' ', '') + '.png';
        this.temperature = json.main.temp + 'ºC';
        this.humidity = json.main.humidity + '%';
        this.description = json.weather[0].description;
        this.windSpeed = json.wind.speed + ' km/h';

        weatherBox.style.display = '';
        weatherDetails.style.display = '';
        weatherBox.classList.add('fadeIn');
        weatherDetails.classList.add('fadeIn');
        container.style.height = '590px';

      })
    }
  },
  computed: {

  },
  created() {

  },
  mounted() {

  },
}

</script>

<template>
<html>

<body>

  <div class="container">
    <div class="search-box">
      <i class="fa-solid fa-location-dot">
      </i>
      <input type="text" v-model="location" placeholder="Enter your location">
      <button class="fa-solid fa-magnifying-glass" type="button" @click="callApi"></button>
    </div>

    <div class="not-found">
      <img src="/public/404.png" alt="">
      <p>Oops! Invalid Location</p>
    </div>

    <div class="weather-box">
      <img :src="weatherImages" alt="">
      <p class="temperature">{{temperature}}</p>
      <p class="description">{{description}}</p>
    </div>

    <div class="weather-details">
      <div class="humidity">
        <i class="fa-solid fa-water"></i>
        <div class="text">
          <span>{{humidity}}</span>
          <p>Humidity</p>
        </div>
      </div>
      <div class="wind">
        <i class="fa-solid fa-wind"></i>
        <div class="text">
          <span>{{windSpeed}}</span>
          <p>Wind speed</p>
        </div>
      </div>
    </div>


  </div>

</body>

</html>

</template>

<style scoped>

</style>