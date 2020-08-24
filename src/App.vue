<style>
  @import 'assets/styles/main.css';
</style>
<template>
  <div id="app" :class="typeof weather.main != 'undefined' && weather.main.temp > 16 ? 'warm' : ''">
    <main>
      <div class="search-box">
        <input 
        type="text" 
        class="search-bar" 
        placeholder="Search..."
        v-model="query"
        @keypress="fetchWeather"
        />
      </div>
      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <div class="location">{{weather.name}}, {{weather.sys.country}}</div>
          <div class="date">{{ dateBuilder() }}</div>
        </div>
        <div class="weather-box">
          <div id="image" :class="weather.main && 
            weather.weather[0].main.toLowerCase ()"> 
          </div>
          <div class="temp">{{Math.round(weather.main.temp)}}<label class="degree">°C</label></div>
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
    </main>
  </div>
</template>
<script>
export default {
  name: 'App',
  data () {
    return {
      api_key: '68830c4c1eac3cc49bf16e4edf01d938',
      url_base: 'https://api.openweathermap.org/data/2.5/',
      query: '',
      weather: {}
    }
  },
  methods: {
    fetchWeather (e){
      if (e.key == "Enter"){
        fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`)
          .then(res => {
            return res.json();
          }).then(this.setResults);
      }
    },
    setResults (results) {
      this.weather = results;
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