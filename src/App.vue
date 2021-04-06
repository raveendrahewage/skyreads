<template>
  <div id="app" :class="typeof weather.main != 'undefined' && weather.main.temp > 16 ? 'warm' : ''">
    <link rel="stylesheet" href="https://bootswatch.com/4/solar/bootstrap.min.css">
    <app-nav></app-nav>
      <fieldset class="mt-4">
        <div class="form-group row d-flex justify-content-center">
          <div class="col-sm-8">
            <input type="text" class="form-control-plaintext text-dark p-3" id="searchbar" placeholder="Type here.." v-model="query" @keypress="fetchWeather">
          </div>
        </div>
      </fieldset>
      <div class="row d-flex justify-content-center mt-4" v-if='typeof weather.main != "undefined"'>
        <div class="col-md-8">
          <div class="card text-dark" id="weather-box">
            <div id="box1" class="card-header text-center display-1"><h3>{{ weather.name }}, {{ weather.sys.country }}</h3><h5>{{ dateBuilder() }}</h5></div>
            <div class="card-body">
              <h1 class="card-title text-center display-1">{{ Math.round(weather.main.temp) }}°c</h1>
              <h3 class="card-text text-center">{{ weather.weather[0].main }}</h3>
            </div>
            <hr>
            <div class="d-flex justify-content-align-content-between mb-2">
                <h4 class="col-md-3">Humidity: {{weather.main.humidity}}</h4>
                <h4 class="col-md-3">Pressure: {{weather.main.pressure}} hpa</h4>
                <h4 class="col-md-3">Wind: {{wind()}}</h4>
                <h4 class="col-md-3">Wind Speed: {{weather.wind.speed}} m/s</h4>
            </div>
          </div>
        </div>
      </div>
    </div>
</template>

<script>
export default {
  name: 'app',
  created() {
    fetch(`${this.url_base}weather?q=colombo&units=metric&APPID=${this.api_key}`)
      .then(res => {
      return res.json();
    }).then(this.setResults);
  },
  data () {
    return {
      api_key: '214f135d8cbecbae09e9c65ba933d4ce',
      url_base: 'https://api.openweathermap.org/data/2.5/',
      query: '',
      weather: {}
    }
  },
  methods: {
    fetchWeather (e) {
      if (e.key == "Enter") {
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
    },
    wind(){
      let deg=this.weather.wind.deg;
      if((deg>=337.5 && deg<=360) || (deg>=0 && deg<22.5))
        return "North"
      else if(deg>=22.5 && deg<=67.5)
        return "North-East";
      else if(deg>=67.5 && deg<=112.5)
        return "East";
      else if(deg>=112.5 && deg<=157.5)
        return "South-East";
      else if(deg>=157.5 && deg<=202.5)
        return "South";
      else if(deg>=202.5 && deg<=247.5)
        return "South-West";
      else if(deg>=247.5 && deg<=292.5)
        return "West";
      else if(deg>=292.5 && deg<=337.5)
        return "North-West";
    }
  }
}
</script>

<style>
    html, body {
      height: 100%;
      margin: 0;
    }
    
    #app{
      height: 100%;
      background-image: url('./assets/img/cold.jpg');
      background-size: cover;
      background-repeat: no-repeat;
      background-position: center center;
      background-attachment: fixed;
      transition: 0.4s;
    }

    #app.warm {
      background-image: url('./assets/img/warm.jpg');
    }

    #searchbar{
      border-radius: 0px 16px 0px 16px;
      border: none;
      appearance: none;
      box-shadow: 0px 8px 8px rgba(143, 143, 143, 0.452);
      outline: none;
      background-color: rgba(255, 255, 255, 0.836);
    }

    #searchbar:focus{
      border-radius: 16px 0px 16px 0px;
      box-shadow: 0px 0px 12px rgba(20, 20, 20, 0.623);
    }

    #weather-box{
      border-radius: 0px 35px 0px 35px;
      box-shadow: 8px 8px 8px rgba(0, 0, 0, 0.459);
      background-color: rgba(255, 255, 255, 0.63);
    }

    #box1{
      border-radius: 0px 35px 0px 35px;
    }

    #appname{
      font-size: 30px;
    }
</style>