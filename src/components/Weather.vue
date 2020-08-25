<template>
  <div class="search-box">
    <input 
    type="text" 
    class="search-bar" 
    placeholder="Search here ..."
    v-model="query"
    @keyup="fetchWeather"
    />
  </div>
  <div class="weather-wrap" v-if="weather.main">
    <div class="content">
      <p class="title">Weather Forecast</p>
      <div class="location-box">
        <div class="location">{{weather.name}}, {{weather.sys.country}}</div>
        <div class="date">{{ dateBuilder() }}</div>
      </div>
    </div>
    
    <div class="weather-box">
      <div id="image" :class="weather.main && 
        weather.weather[0].main.toLowerCase ()"> 
      </div>
      <div class="temp">{{Math.round(weather.main.temp)}}<label class="degree"></label></div>
      <div class="weather"><span id="weather">{{weather.weather[0].main}}</span></div>   
      <div class="more-info">
        <div class="item">{{weather.wind.speed}} km/h
          <div class="description">Wind</div>
        </div>
        <div class="item">{{weather.main.humidity}}%
          <div class="description">Humidity</div>
        </div>
        <div class="item">{{Math.round(weather.main.temp_max)}}°
          <div class="description">Maximum</div>
        </div>
      </div>
    </div>
  </div> 
  <div class="error" v-else-if="error != null">
    <img src="../assets/img/cry.svg">
    <h1>{{error}}</h1>
  </div>
  <div class="landing-page" v-else>
    <div class="container">
      <div class="content">
        <h4>Weather APP</h4>
        <h1>Welcome</h1>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Weather',
  data () {
    return {
      api_key: '68830c4c1eac3cc49bf16e4edf01d938',
      url_base: 'https://api.openweathermap.org/data/2.5/',
      query: '',
      weather: {},
      error: null
    }
  },
  methods: {
    async fetchWeather (e){
      if (e.key == "Enter"){
        let request = await fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`) 
        if (request.status == 404) {
          this.error = 'City not found';
        } else {
          this.error = null;  
        }
        this.weather = await request.json()
      }
    },
    dateBuilder () {
      let d = new Date();
      let months = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];
      let days = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"];
      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();
      return `${day} ${date} ${month} ${year}`;
    }
  }
}
</script>