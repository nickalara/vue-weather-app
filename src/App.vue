<template>
  <div id="app"
    :class="typeof weather.main != 'undefined' && weather.main.temp > 60 ? 'warm' : ''">
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
          <div class="location">{{ weather.name }}, {{ weather.sys.country }}</div>
          <div class="date">{{ today() }}</div>
        </div>

        <div class="weather-box">
          <div class="temp">{{ Math.round(weather.main.temp) }}°F</div>
          <div class="weather">{{ weather.weather[0].main }}</div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
export default {
  name: 'app',
  data () {
    return {
      api_key: '8963751014afb558101ea34e898a1996',
      url_base: 'http://api.openweathermap.org/data/2.5/',
      query: '',
      weather: {}
    }
  },
  methods: {
    fetchWeather (e) {
      if (e.key == "Enter") {
        fetch(`${this.url_base}weather?q=${this.query}&units=imperial&APPID=${this.api_key}`)
          .then(res => {
            return res.json();
          }).then(this.setResults);
      }
    },
    setResults (results) {
      this.weather = results;
    },
    today () {
      let today = new Date();
      let month = ["January", "February", "March", "April", "May", "June", "July",
      "August", "September", "October", "November", "December"];
      let days = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday",
      "Saturday"]

      let date = days[today.getDay()] + " " + month[today.getMonth()] + " " +
      today.getDate() + ", " + today.getFullYear();
      
      return date;
    }
  }
}
</script>

<style>
  * {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
  }

  body {
    font-family: 'montserrat', sans-serif;
  }

  #app {
    background-image: url('./assets/cold-bg.jpg');
    background-size:  cover;
    background-position: bottom;
    transition: 0.4s;
  }

  #app.warm {
    background-image: url('./assets/warm-bg.jpg');
    background-size:  cover;
    background-position: bottom;
    transition: 0.4s;
  }

  main {
    min-height: 100vh;
    padding: 25px;

    background-image: linear-gradient(to bottom, rgba(0, 0, 0, 0.25), rgba(0, 0, 0, 0.75));
  }

  .search-box {
    width: 100%;
    margin-bottom: 30px;
  }

  .search-box .search-bar {
    display: block;
    width: 100%;
    padding: 15px;

    color: #313131;
    font-size: 20px;

    appearance: none;
    border: none;
    outline: none;
    background: none;

    box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
    background-color: rgba(255, 255, 255, 0.5);
    border-radius: 12px 12px 12px 12px;
    transition: 0.4s;
  }

  .search-box .search-bar:focus {
    box-shadow: 0px 0px 20px rgba(0, 0, 0, 0.25);
    background-color: rgba(255, 255, 255, 0.75);
  }

  .location-box .location{
    color: #FFF;
    font-size: 32px;
    font-weight: 500;
    text-align: center;
    text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
  }

  .location-box .date{
    color: #FFF;
    font-size: 20px;
    font-weight: 300;
    text-align: center;
    text-shadow: 1px 2px rgba(0, 0, 0, 0.25);
  }

  .weather-box {
    text-align: center;
  }

  .weather-box .temp {
    display: inline-block;
    padding: 20px 25px;
    color: #FFF;
    font-size: 100px;
    font-weight: 500;

    text-shadow: 4px 4px rgba(0, 0, 0, 0.25);
    background-color:rgba(255, 255, 255, 0.25);
    border-radius: 16px;
    margin: 30px 0px 10px 0px;
  }

  .weather-box .weather {
    padding: 20px 25px;
    color: #FFF;
    font-size: 48px;
    font-weight: 600;
    text-shadow: 3px 3px rgba(0, 0, 0, 0.25);
  }
</style>
