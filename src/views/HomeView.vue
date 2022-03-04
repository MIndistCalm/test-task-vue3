<template>
  <div
    class="home"
    :class="
      typeof weather.main != 'undefined' && weather.main.temp > 16 ? 'warm' : ''
    "
  >
    <main>
      <div class="search-box">
        <input
          type="text"
          class="search-bar"
          placeholder="Введите город или страну"
          v-model="query"
          @keypress="fetchWeather"
        />
      </div>
      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <div class="location">
            {{ weather.name }}, {{ weather.sys.country }}
          </div>
          <div class="date">
            {{ dateBuilder() }} {{ hours }}:{{ minutes }}:{{ seconds }}
          </div>
        </div>
      </div>
      <div class="weather-box" v-if="typeof weather.main != 'undefined'">
        <div class="weather-icon">
          <div class="icon" v-html="weatherIcon(weather.weather[0].main)"></div>
        </div>
        <div class="weather">{{ weather.weather[0].main }}</div>
        <div class="temp">{{ Math.round(weather.main.temp) }}°C</div>
        <div></div>
      </div>
    </main>
  </div>
</template>

<script>
//import { LocationMarkerIcon, MoonIcon, SunIcon } from "@heroicons/vue/outline";
export default {
  name: "HomeView",
  components: {},
  data() {
    return {
      api_key: "4bddfb4ee613021b47dc87f37a39069a",
      url_base: "https://api.openweathermap.org/data/2.5/",
      query: "Tomsk",
      weather: {},
      hours: 0,
      minutes: 0,
      seconds: 0,
    };
  },
  methods: {
    fetchWeather(e) {
      if (e.key == "Enter") {
        fetch(
          `${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`
        )
          .then((res) => {
            return res.json();
          })
          .then(this.setResults);
      }
    },
    setResults(results) {
      this.weather = results;
      console.log(this.data);
    },
    checkSingleDigit(digit) {
      return ("0" + digit).slice(-2);
    },
    setTime() {
      setInterval(() => {
        const date = new Date();
        this.hours = date.getHours();
        this.minutes = this.checkSingleDigit(date.getMinutes());
        this.seconds = this.checkSingleDigit(date.getSeconds());
      }, 1000);
    },
    dateBuilder() {
      let d = new Date();
      let months = [
        "January",
        "February",
        "March",
        "April",
        "May",
        "June",
        "July",
        "August",
        "September",
        "October",
        "November",
        "December",
      ];
      let days = [
        "Sunday",
        "Monday",
        "Tuesday",
        "Wednesday",
        "Thursday",
        "Friday",
        "Saturday",
      ];
      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();
      return `${day} ${date} ${month} ${year}`;
    },
    weatherIcon(weather) {
      let clouds =
        '<svg version="1.1" id="Layer_1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" x="0px" y="0px" viewBox="0 0 485 485" style="enable-background:new 0 0 485 485;" xml:space="preserve"><path d="M485,214.04c0-39.199-28.706-71.819-66.201-77.947c-11.772-51.31-57.695-88.639-111.478-88.639c-48.103,0-90.978,30.346-107.367,74.864c-55.911,12.776-100.346,57.227-112.082,114.524C38.375,243.191,0,285.59,0,336.783c0,55.561,45.202,100.763,100.763,100.763h264.601c55.561,0,100.763-45.202,100.763-100.763c0-20.852-6.369-40.242-17.261-56.334C471.077,266.202,485,241.528,485,214.04z M307.322,77.454c42.764,0,78.789,31.97,83.798,74.366l1.564,13.24h13.332c27.01,0,48.984,21.973,48.984,48.98c0,18.897-10.598,35.657-27.05,43.832c-14.023-11.145-31.071-18.641-49.695-21.031c-13.858-67.654-73.302-117.397-143.4-118.233C249.82,93.616,277.171,77.454,307.322,77.454z M365.364,407.546H100.763C61.744,407.546,30,375.802,30,336.783s31.746-70.763,70.768-70.763H114.1l1.564-13.24c7.018-59.399,57.489-104.191,117.399-104.191s110.381,44.792,117.399,104.191l1.564,13.24h13.332c39.021,0,70.768,31.744,70.768,70.763S404.383,407.546,365.364,407.546z"/><g></g><g></g><g></g><g></g><g></g><g></g><g></g><g></g><g></g><g></g><g></g><g></g><g></g><g></g><g></g></svg>';
      let clear =
        '<svg class="h-24 w-24 text-purple-500"  viewBox="0 0 24 24"  fill="none"  stroke="currentColor"  stroke-width="2"  stroke-linecap="round"  stroke-linejoin="round">  <circle cx="12" cy="12" r="5" />  <line x1="12" y1="1" x2="12" y2="3" />  <line x1="12" y1="21" x2="12" y2="23" />  <line x1="4.22" y1="4.22" x2="5.64" y2="5.64" />  <line x1="18.36" y1="18.36" x2="19.78" y2="19.78" />  <line x1="1" y1="12" x2="3" y2="12" />  <line x1="21" y1="12" x2="23" y2="12" />  <line x1="4.22" y1="19.78" x2="5.64" y2="18.36" />  <line x1="18.36" y1="5.64" x2="19.78" y2="4.22" /></svg>';
      let snow =
        '<svg class="h-24 w-24 text-purple-500"  viewBox="0 0 24 24"  fill="none"  stroke="currentColor"  stroke-width="2"  stroke-linecap="round"  stroke-linejoin="round">  <path d="M20 17.58A5 5 0 0 0 18 8h-1.26A8 8 0 1 0 4 16.25" />  <line x1="8" y1="16" x2="8.01" y2="16" />  <line x1="8" y1="20" x2="8.01" y2="20" />  <line x1="12" y1="18" x2="12.01" y2="18" />  <line x1="12" y1="22" x2="12.01" y2="22" />  <line x1="16" y1="16" x2="16.01" y2="16" />  <line x1="16" y1="20" x2="16.01" y2="20" /></svg>';
      let mist =
        '<svg version="1.1" id="Capa_1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" x="0px" y="0px"viewBox="0 0 217.43 217.43" style="enable-background:new 0 0 217.43 217.43;" xml:space="preserve"><g><path d="M144.797,47.095c0-4.142-3.358-7.5-7.5-7.5H7.5c-4.142,0-7.5,3.358-7.5,7.5c0,4.142,3.358,7.5,7.5,7.5h129.797 C141.439,54.595,144.797,51.237,144.797,47.095z"/><path d="M209.93,70.405H58.632c-4.142,0-7.5,3.358-7.5,7.5s3.358,7.5,7.5,7.5H209.93c4.142,0,7.5-3.358,7.5-7.5S214.072,70.405,209.93,70.405z"/><path d="M174.53,116.214c4.142,0,7.5-3.358,7.5-7.5c0-4.142-3.358-7.5-7.5-7.5H22.446c-4.142,0-7.5,3.358-7.5,7.5c0,4.142,3.358,7.5,7.5,7.5H174.53z"/><path d="M199.441,132.024H47.619c-4.142,0-7.5,3.358-7.5,7.5s3.358,7.5,7.5,7.5h151.822c4.142,0,7.5-3.358,7.5-7.5S203.583,132.024,199.441,132.024z"/><path d="M125.759,162.835H25.068c-4.142,0-7.5,3.358-7.5,7.5c0,4.142,3.358,7.5,7.5,7.5h100.69c4.142,0,7.5-3.358,7.5-7.5 C133.259,166.193,129.901,162.835,125.759,162.835z"/></g><g></g><g></g><g></g><g></g><g></g><g></g><g></g><g></g><g></g><g></g><g></g><g></g><g></g><g></g><g></g></svg>';
      let wind =
        '<svg class="h-24 w-24 text-purple-500"  viewBox="0 0 24 24"  fill="none"  stroke="currentColor"  stroke-width="2"  stroke-linecap="round"  stroke-linejoin="round">  <path d="M9.59 4.59A2 2 0 1 1 11 8H2m10.59 11.41A2 2 0 1 0 14 16H2m15.73-8.27A2.5 2.5 0 1 1 19.5 12H2" /></svg>';
      let rain =
        '<svg version="1.1" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 512 512" xmlns:xlink="http://www.w3.org/1999/xlink" enable-background="new 0 0 512 512"><g><g><path d="m396.7,241.8c-8.3-67.6-68.2-120.3-140.7-120.3s-132.4,52.6-140.7,120.3c-51,12.4-91.4,51.6-103.3,101.8-2.7,11.5-1,46.5 39.7,48.4h408.5c40.7-2.8 42.4-36.9 39.7-48.4-11.8-50.1-52.3-89.3-103.2-101.8zm-344.5,109.3c9.9-38.1 43.8-66.9 85-71.8 10.3-1.2 18-9.9 18-20.3 0-0.5 0-1 0-1.5 0.3-52.5 45.4-95.1 100.8-95.1 55.4,0 100.6,42.6 100.8,95.1 0,0.4 0,0.9-0.1,1.2-0.1,10.4 7.6,19.3 18,20.6 41.2,5 75.1,33.8 85,71.9l-407.5-.1z"/><path d="m31.4,240c11.3,0 20.4-9.1 20.4-20.4 0-25.9 19.7-49.8 50.1-60.8 10.1-3.7 15.7-14.6 12.6-25-1.5-5.2-2.3-10-2.3-14.9 0-37 38.6-67.1 86-67.1 32.8,0 63.3,15.1 77.6,38.5 5.2,8.5 15.9,12 25.1,8.2 12.5-5.1 25.8-7.7 39.6-7.7 46.4,0 85,29.5 85.9,65.7 0.2,6.1 3.1,11.8 7.9,15.6 16.5,12.8 25.6,29.7 25.6,47.5 0,11.3 9.1,20.4 20.4,20.4 11.3,0 20.4-9.1 20.4-20.4 0-27.4-12.4-53.9-34.3-73.8-7-54.5-60.3-95.8-126-95.8-13.6,0-26.9,1.8-39.8,5.5-23.3-27.8-61.1-44.5-102.3-44.5-70,0-126.9,48.4-126.9,107.9 0,2.8 0.1,5.6 0.4,8.5-37.4,19.3-60.8,54-60.8,92.2 0,11.2 9.1,20.4 20.4,20.4z"/><path d="m108.3,410.8c-11.3,0-20.4,9.1-20.4,20.4v10.6c0,11.3 9.1,20.4 20.4,20.4s20.4-9.1 20.4-20.4v-10.6c-2.84217e-14-11.2-9.1-20.4-20.4-20.4z"/><path d="m249.4,410.8c-11.3,0-20.4,9.1-20.4,20.4v10.6c0,11.3 9.1,20.4 20.4,20.4 11.3,0 20.4-9.1 20.4-20.4v-10.6c0.1-11.2-9.1-20.4-20.4-20.4z"/><path d="m390.6,410.8c-11.3,0-20.4,9.1-20.4,20.4v10.6c0,11.3 9.1,20.4 20.4,20.4 11.3,0 20.4-9.1 20.4-20.4v-10.6c0-11.2-9.1-20.4-20.4-20.4z"/><path d="m184.3,449.6c-11.3,0-20.4,9.1-20.4,20.4v10.6c0,11.3 9.1,20.4 20.4,20.4 11.3,0 20.4-9.1 20.4-20.4v-10.6c-2.84217e-14-11.3-9.2-20.4-20.4-20.4z"/><path d="m327.7,449.6c-11.3,0-20.4,9.1-20.4,20.4v10.6c0,11.3 9.1,20.4 20.4,20.4 11.3,0 20.4-9.1 20.4-20.4v-10.6c5.68434e-14-11.3-9.1-20.4-20.4-20.4z"/></g></g></svg>';
      if (weather == "Clouds") return clouds;
      else if (weather == "Clear") return clear;
      else if (weather == "Snow") return snow;
      else if (weather == "Mist") return mist;
      else if (weather == "Wind") return wind;
      else if (weather == "Rain") return rain;
      else return weather;
    },
  },
  mounted() {
    this.setTime();
  },
  darkMode() {
    this.darkmode = !this.darkmode;
  },
};
</script>

<style scoped>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
body {
  font-family: "montserrat", sans-serif;
}
.home {
  background-image: radial-gradient(#a8d0fa, #3b96f7);
  background-size: cover;
  background-position: bottom;
  transition: 0.6s;
  /* width: 40vw; */
  /* align-self: center; */
}
.home.warm {
  background-image: radial-gradient(#8a5697, #42104e);
}
main {
  min-height: 100vh;
  padding: 25px;

  background-image: linear-gradient(
    to bottom,
    rgba(0, 0, 0, 0.25),
    rgba(0, 0, 0, 0.75)
  );
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

  box-shadow: 0 0 8px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.5);
  border-radius: 0 16px 0 16px;
  transition: 0.4s;
}

.search-box .search-bar:focus {
  box-shadow: 0 0 8px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.75);
  border-radius: 16px 0 16px 0;
}

.location-box .location {
  color: #fff;
  font-size: 32px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}

.location-box .date {
  color: #fff;
  font-size: 20px;
  font-weight: 300;
  text-align: center;
  text-style: italic;
}

.weather-box .temp {
  display: inline-block;
  padding: 10px 25px;
  color: white;
  font-size: 102px;
  font-weight: 900;

  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.25);
  border-radius: 16px;
  margin: 30px 0;

  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
.weather-box .weather {
  color: #fff;
  font-size: 48px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

.search-box {
  margin-bottom: 20px;
  position: relative;
}

input.search-bar {
  text-align: center;
  box-shadow: 1px 1px 2px #00000070;
  text-transform: capitalize;
  padding-left: 100px;
  padding-right: 20px;
  width: calc(100% - 85px);
  display: inline-block;
}

.location-box .date {
  font-weight: 600;
}

.search-box > svg {
  position: absolute;
  top: 10px;
  left: 20px;
  opacity: 0.5;
}

.weather-box {
  background: #ffffff7a;
  display: inline-block;
  padding: 20px 30px;
  border-radius: 10px;
  margin-top: 20px;
  box-shadow: 0px 0px 12px #0000003b;
  min-width: 300px;
}

.weather-box .temp {
  font-weight: bold;
  text-shadow: 1px 1px #afafaf;
  margin-top: 8px;
}

.weather-box svg {
  display: inline-block;
}

.weather-icon {
  display: flex;
  align-items: flex-end;
  justify-content: center;
}

.weather-icon .weather {
  background: #fff;
  padding: 3px 10px;
  border: 1px solid #c8c8c8;
  line-height: 1;
  border-radius: 10px;
  font-weight: 600;
  margin-left: -10%;
  font-size: 15px;
  margin-bottom: 5%;
}
.icon {
  width: 200px;
  height: 200px;
}
</style>
