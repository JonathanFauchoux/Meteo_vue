<template>
  <div id="app" :class="typeof weather.main != 'undefined' && weather.main.temp > 16 ? 'warm' : ''">
    <main>
      <div class="search-box">
        <input
          type="text"
          class="search-bar"
          placeholder="Votre ville"
          v-model="query"
          @keypress="fetchWeather"
        >
      </div>

      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <div class="location">{{ weather.name }}, {{ weather.sys.country }}</div>
          <div class="date">{{ dateBuilder() }}</div>
        </div>

        <div class="weather-box">
          <div class="temp">{{ Math.round(weather.main.temp) }}°</div>
          <div class="weather">{{ weather.weather[0].description }}</div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
export default {
  name: "app",
  data() {
    return {
      api_key: "6121c27282c1bbf2da89edb69f60cdaa",
      url_base: "https://api.openweathermap.org/data/2.5/",
      query: "",
      weather: {}
    };
  },
  methods: {
    fetchWeather(e) {
      if (e.key === "Enter") {
        this.weather= {}
        fetch(
          `${this.url_base}weather?q=${this.query}&units=metric&APPID=${
            this.api_key
          }&lang=fr`
        )
          .then(res => {
            return res.json();
          })
          .then(this.setResults);
          this.query = "";
      }
    },
    setResults(results) {
      this.weather = results;
      console.log(results);
    },
    dateBuilder() {
      let d = new Date();
      let months = [
        "Janvier",
        "Fevrier",
        "Mars",
        "Avril",
        "Mai",
        "Juin",
        "Juillet",
        "Aout",
        "Septembre",
        "Octobre",
        "Novembre",
        "Decembre"
      ];
      let days = [
        "Dimache",
        "Lundi",
        "Mardi",
        "Mercredi",
        "Jeudi",
        "Vendredi",
        "Samedi"
      ];
      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();
      return `${day} ${date} ${month} ${year}`;
    }
  }
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
body {
  font-family: "montserrat", sans-serif;
}
#app {
  background-image: url("https://images.unsplash.com/photo-1494280986787-c49b328829dd?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1500&q=80");
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}

main {
  width: 100%;
  height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
  padding: 1.5rem;
  background-image: linear-gradient(
    to bottom,
    rgba(255, 255, 0, 0.25),
    rgba(0, 0, 0, 0.85)
  );
 
  
}
.search-box {
  justify-self: start;
  width: 100%;
  margin-bottom: 2rem;
  display: flex;
  justify-content: center;
}
.search-box .search-bar {
  display: block;
  width: 100%;
  padding: 1rem;

  color: #313131;
  font-size: 20px;
  appearance: none;
  border: none;
  outline: none;
  background: none;
  box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.65);
  border-radius: 0px 16px 0px 16px;
  transition: 0.4s;
}
.search-box .search-bar:focus {
  box-shadow: 0px 0px 16px rgba(255, 255, 255, 0.75);
  background-color: rgba(255, 255, 255, 0.85);
  border-radius: 16px 0px 16px 0px;
}
/*.location-box{
  padding-bottom: 6rem;
}*/

.location-box .location {
  color: #FFF;
  font-size: 2.5rem;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}
 .date {
  color: #FFF;
  font-size: 1.3rem;
  font-weight: 300;
  font-style: italic;
  text-align: center;
}
.weather-wrap{
  width: 100%;
  display: flex;
  flex-direction: column;
  justify-items: center;
}
.weather-box {
  text-align: center;
  width: 100%;
}
.weather-box .temp {
  display: inline-block;
  padding: 10px 25px;
  color: #FFF;
  white-space: nowrap;
  font-size: 5rem;
  font-weight: 900;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.25);
  border-radius: 16px;
  margin: 2rem 0;
  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
.weather-box .weather {
  color: #FFF;
  font-family: "Roboto";
  font-size: 2rem;
  font-weight: 100;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);

  border-top: 1px solid rgba(255, 255, 255, 0.25);
  border-bottom: 1px solid rgba(255, 255, 255, 0.25);
  width: 100%;
  padding: 1rem 0;
}

.search-bar::placeholder {
  color: Black;
}
.search-bar:focus::placeholder {
  color: transparent;
}
 @media screen and (min-width: 768px) and (max-width: 1024px) {
    main{
      justify-content: space-evenly;
    }
    .search-box .search-bar{
      width: 80%;
      padding: 1.2rem !important;
      font-size: 1.5rem;
    }
    .location-box{
      margin-top: -10rem;
      padding-bottom: 10rem;;
    }
    .weather{
      font-size: 3.9rem !important;
      /*border: 1px solid red;*/
    }
  }
  @media screen and (min-width: 1024px) {
    main{
      justify-content: center;
    }
    .search-box .search-bar{
      width: 80%;
      padding: 1.2rem !important;
      font-size: 1.5rem;
    }
    .location-box{
      margin-top: 1rem;
      padding-bottom: 10rem;;
    }
    .weather{
      font-size: 3rem !important;
      width: 88%;
      /*border: 1px solid red;*/
    }
  }
</style>