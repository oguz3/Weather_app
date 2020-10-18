<template>
  <div id="app" :class="typeof weather.main != 'undefined' && weather.main.temp > 16 ? 'sun' : ''">
      <main>
        <div class="search__box">
          <input 
            type= "text" 
            class= "search__bar" 
            placeholder= "Search..."
            v-model= "query"
            @keypress= "fetchWeather"
          />
        </div>
        <div class="weather__details">
          <div class="location">
            <div class="city">{{ weather.name }}, {{ weather.sys.country }}</div>
            <div class="date">{{getDate()}}</div>
          </div>
          <div class="weather">
            <div class="temp">{{ Math.round(weather.main.temp) }}°c</div>
            <div class="case">{{ weather.weather[0].main}}</div>
          </div>
        </div>
        <div class="img__attribute">
          <a href="https://www.vecteezy.com/free-vector/exact">Exact Vectors by Vecteezy</a> ||
          <a href="https://www.vecteezy.com/free-vector/winter">Winter Vectors by Vecteezy</a>
        </div>
      </main>
  </div>
</template>

<script>
export default {
  name: 'App',
  data (){
    return {
      api_key: '51b3ccab32783c38045ab7a555b13531',
      url_base: 'https://api.openweathermap.org/data/2.5/',
      query: '',
      weather: {
        'weather': [{'main': 'Rain'}],
        'main': {'temp': 26},
        'name': 'Denizli',
        'sys' : {'country': 'TR'}
      }
    }
  },
  methods: {
    fetchWeather (e){
      if (e.key == "Enter") {
        fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`)
          .then(res => {
            return res.json();
          }).then(this.setResults);
      }
    },
    setResults (results){
      this.weather = results;
    },
    getDate(){
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

<style>
*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
body{
  font-family: 'montserrat', sans-serif;
}
#app{
  background-image: url('./assets/winter.svg');
  background-size: cover;
  background-position: bottom;
  transition: .4s;
}
#app.sun{
    background-image: url('./assets/sunny.svg');
}
main{
  display: flex;
  flex-direction: column;
  min-height: 100vh;
  padding: 25px;
  background-image: linear-gradient(to bottom, rgba(0, 0, 0, 0.25), rgba(0, 0, 0, 0.50));
}
.search__box{
  width: 100%;
  margin-bottom: 30px;
}
.search__box .search__bar{
  display: block;
  width: 100%;
  padding: 15px;
  color: #313131;
  font-size: 20px;
  appearance: none;
  border: none;
  outline: none;
  background: none;
  background-color: rgba(255, 255, 255, 0.50);
  border-radius: 0px 16px 0px 16px;
  box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
  transition: .4s;
}
.search__box .search__bar:focus{
  background-color: rgba(255, 255, 255, 0.75);
  box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.50);
  border-radius: 0px;
}
.weather__details{
  margin-top: auto;
  margin-bottom: auto;
}
.location .city{
  color: #fff;
  font-size: 64px;
  font-weight: 500;
  text-align: center;
  text-shadow:  1px 3px rgba(0, 0, 0, 0.25);
}
.location .date{
  color: #fff;
  font-size: 40px;
  font-weight: 300;
  text-align: center;
  font-style: italic;
}
.weather{
  text-align: center;
}
.weather .temp{
  display: inline-block;
  padding: 10px 25px;
  color: #fff;
  font-size: 153px;
  font-weight: 900;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.25);
  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  border-radius: 16px;
  margin: 30px 0px;
}
.weather .case {
  color: #FFF;
  font-size: 72px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
.img__attribute{
  margin-top: auto;
  margin-bottom: -20px;
  margin-left: -20px;
  color: rgba(255, 255, 255, 0.473);
}
.img__attribute a{
  color: rgba(255, 255, 255, 0.473);
  font-size: 12px;
}
@media screen and (max-width: 992px) {
  .location .city{
    font-size: 32px;
  }
  .location .date{
    font-size: 20px;
  }
  .weather .temp{
    font-size: 102px;
  }
  .weather .case {
    font-size: 48px;
  }
  .weather__details{
    margin-top: 50px;
    margin-bottom: auto;
  }
}
</style>
