<script>
import 'primeicons/primeicons.css'
export default {
  name: 'app',
  data() {
    return {
      query: '',
      error: '',
      weather: {}
    }
  },
  methods: {
    async fetchWeather() {
      try {
        this.error = ''
        const res = await fetch(
          `https://api.openweathermap.org/data/2.5/weather?q=${this.query}&units=metric&appid=348cbe9a898ab7edf882a27e273ac4c1
`
        )
        const data = await res.json()
        if (data.message == 'city not found') {
          this.error = 'city not found'
        } else {
          this.weather = data
        }
      } catch (error) {
        console.log({ msg: 'error while trying to fetch data based on query', error })
      }
    },
    dateBuilder() {
      let d = new Date()
      let months = [
        'January',
        'February',
        'March',
        'April',
        'May',
        'June',
        'July',
        'August',
        'September',
        'October',
        'November',
        'December'
      ]
      let days = ['Sunday', 'Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday']
      let day = days[d.getDay()]
      let date = d.getDate()
      let month = months[d.getMonth()]
      let year = d.getFullYear()

      return `${day} ${date} ${month} ${year}`
    }
  }
}
</script>

<template>
  <div
    id="app"
    :class="typeof this.weather.main !== 'undefined' && this.weather.main.temp > 20 ? 'warm' : ''"
  >
    <main>
      <div class="error-searchbar-container">
        <div class="search-box">
          <input type="text" class="search-bar" placeholder="Search.." v-model="query" />
          <span class="pi pi-search" @click="fetchWeather"></span>
        </div>
        <h5 v-if="error !== ''" class="error-show">{{ error }}</h5>
      </div>
      <div class="weather-wrap" v-if="typeof this.weather?.name != 'undefined'">
        <div class="location-box">
          <p class="location">{{ this.weather?.name }} {{ this.weather?.sys?.country }}</p>
          <p class="date">{{ this.dateBuilder() }}</p>
        </div>
        <div class="weather-box">
          <p class="temp">{{ this.weather?.main?.temp?.toFixed(0) }}&deg;C</p>
          <p class="weather">
            {{ this.weather?.name ? this.weather?.weather[0]?.main : '' }}
          </p>
        </div>
      </div>
    </main>
  </div>
</template>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Montserrat:ital,wght@0,100;0,200;0,300;0,400;0,500;0,700;0,800;0,900;1,100;1,200;1,300;1,400;1,500;1,600;1,700;1,800;1,900&display=swap');
/* font-family: 'Montserrat', sans-serif; */
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
body {
  font-family: 'Montserrat', sans-serif;
}
#app {
  background-image: url('./assets/cold-bg.jpg');
  background-size: contain;
  background-position: bottom;
  transition: 0.4s;
}
.error-searchbar-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: flex-start;
}
.error-show {
  color: white;
  font-family: 'Montserrat', sans-serif;
  margin-top: -1rem;
  font-style: italic;
  font-weight: 400;
}
#app.warm {
  background-image: url('./assets/warm-bg.jpg');
}
main {
  height: 100vh;

  padding: 25px;
  font-family: 'Montserrat', sans-serif;
  background: linear-gradient(to bottom, rgba(0, 0, 0, 0.25), rgba(0, 0, 0, 0.75));
}
.search-box {
  width: 100%;
  margin-bottom: 30px;
  font-family: 'Montserrat', sans-serif;
  display: flex;
  align-items: center;
  gap: 2rem;
}
.search-box :nth-child(2) {
  padding: 15px;

  background-color: aliceblue;
  border-radius: 50%;
  border: 1px solid aliceblue;
  transition: all ease-in-out 0.5s;
}
.search-box :nth-child(2):hover {
  background-color: transparent;
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
  border-radius: 0px 16px 0px 16px;
  transition: 0.4s;
  font-family: 'Montserrat', sans-serif;
}
.search-box .search-bar:focus {
  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.75);
  border-radius: 16px 0px 16px 0px;
}
.location-box .location {
  color: white;
  font-size: 32px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
  font-family: 'Montserrat', sans-serif;
}
.location-box .date {
  color: white;
  font-size: 20px;
  font-weight: 300;
  font-style: italic;
  text-align: center;
  font-family: 'Montserrat', sans-serif;
}
.weather-box {
  text-align: center;
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
  margin: 30px 0px;
  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  font-family: 'Montserrat', sans-serif;
}
.weather-box .weather {
  color: white;
  font-size: 48px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  font-family: 'Montserrat', sans-serif;
}
</style>
