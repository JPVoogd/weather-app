<!--suppress ALL -->
<template>
  <div id="app">
    <main>
      <h1 class="title">Anemoi's Forecast</h1>
      <div class="search-box">
        <input type="text" class="search-bar" v-model="query" placeholder="Enter city name"
          @keydown.enter="fetchLocation" />
      </div>

      <div class="weather-wrap" v-if="weatherData">

        <div class="cards">
          <div class="card location">
            <div class="location-card">{{ location[0].name }}, {{ location[0].country }}</div>
            <div class="time">{{ localTime(weatherData.current.dt, weatherData.timezone_offset) }}</div>
            <div class="date">{{ getLocalDate(weatherData.current.dt, weatherData.timezone_offset) }}</div>
          </div>

          <div class="card weather">
            <div>
              <div class="first-section">
                <div class="temps">
                  <h2 class="temp">{{ Math.round(weatherData.current.temp) }}°C</h2>
                  <h3 class="temp-feel">Feels like: {{ Math.round(weatherData.current.feels_like) }}°C</h3>
                </div>

                <div class="sun">
                  <div class="sunrise">
                    <img src="./assets/weather/wi-sunrise.svg" alt="Sunrise Icon">
                    <div>
                      <h4>Sunrise</h4>
                      <p>{{ localTime(weatherData.current.sunrise, weatherData.timezone_offset) }}</p>
                    </div>
                  </div>

                  <div class="sunset">
                    <img src="./assets/weather/wi-sunset.svg" alt="Sunset Icon">
                    <div>
                      <h4>Sunset </h4>
                      <p>{{ localTime(weatherData.current.sunset, weatherData.timezone_offset) }}</p>
                    </div>
                  </div>
                </div>
              </div>

            </div>
            <div class="middle-section">
              <img :src="require(`@/assets/weather/${getWeatherIcon(weatherData.current.weather[0].icon)}`)"
                alt="Weather Icon">
              <p>{{ weatherData.current.weather[0].description }}</p>
            </div>

            <div class="last-section">
              <div>
                <img src="./assets/weather/wi-humidity.svg" alt="Humidity Icon">
                <p>{{ weatherData.current.humidity }}%</p>
                <p>humidity</p>
              </div>

              <div>
                <img src="./assets/weather/wi-windy.svg" alt="Wind speed Icon">
                <p>{{ Math.round(weatherData.current.wind_speed * 3.16) }}km/h</p>
                <p>Wind Speed</p>
              </div>

              <div>
                <img src="./assets/weather/wi-barometer.svg" alt="Barometer Icon">
                <p>{{ weatherData.current.pressure }}hPa</p>
                <p>Pressure</p>
              </div>

              <div>
                <img src="./assets/weather/wi-wind-deg.svg" alt="Wind degrees Icon">
                <p>{{ weatherData.current.wind_deg }}</p>
                <p>Degrees</p>
              </div>
            </div>
          </div>
        </div>

        <div class="cardsTwo">
          <div class="card forecast-day">
            <h2>5 Days Forecast:</h2>

            <div class="forecast-days">
              <img :src="require(`@/assets/weather/${getWeatherIcon(weatherData.daily[0].weather[0].icon)}`)"
                alt="Weather Icon">
              <p class="item">{{ Math.round(weatherData.daily[0].temp.day) }}°C</p>
              <p class="item">{{ getLocalDate(weatherData.daily[0].dt, weatherData.timezone_offset) }}</p>
            </div>

            <div class="forecast-days">
              <img :src="require(`@/assets/weather/${getWeatherIcon(weatherData.daily[1].weather[0].icon)}`)"
                alt="Weather Icon">
              <p class="item">{{ Math.round(weatherData.daily[1].temp.day) }}°C</p>
              <p class="item">{{ getLocalDate(weatherData.daily[1].dt, weatherData.timezone_offset) }}</p>
            </div>

            <div class="forecast-days">
              <img :src="require(`@/assets/weather/${getWeatherIcon(weatherData.daily[2].weather[0].icon)}`)"
                alt="Weather Icon">
              <p class="item">{{ Math.round(weatherData.daily[2].temp.day) }}°C</p>
              <p class="item">{{ getLocalDate(weatherData.daily[2].dt, weatherData.timezone_offset) }}</p>
            </div>

            <div class="forecast-days">
              <img :src="require(`@/assets/weather/${getWeatherIcon(weatherData.daily[3].weather[0].icon)}`)"
                alt="Weather Icon">
              <p class="item">{{ Math.round(weatherData.daily[3].temp.day) }}°C</p>
              <p class="item">{{ getLocalDate(weatherData.daily[3].dt, weatherData.timezone_offset) }}</p>
            </div>

            <div class="forecast-days">
              <img :src="require(`@/assets/weather/${getWeatherIcon(weatherData.daily[4].weather[0].icon)}`)"
                alt="Weather Icon">
              <p class="item">{{ Math.round(weatherData.daily[4].temp.day) }}°C</p>
              <p class="item">{{ getLocalDate(weatherData.daily[4].dt, weatherData.timezone_offset) }}</p>
            </div>

          </div>

          <div class="card forecast-hourly">
            <h2>Hourly Forecast:</h2>

            <div class="forecast-card">
              <div class="forecast-hours">
                <p>{{ localTime(weatherData.hourly[1].dt, weatherData.timezone_offset) }}</p>
                <img :src="require(`@/assets/weather/${getWeatherIcon(weatherData.hourly[1].weather[0].icon)}`)"
                  alt="Weather Icon">
                <p>{{ Math.round(weatherData.hourly[1].temp) }}°C</p>
                <p>{{ weatherData.hourly[1].weather[0].description }}</p>
              </div>

              <div class="forecast-hours">
                <p>{{ localTime(weatherData.hourly[2].dt, weatherData.timezone_offset) }}</p>
                <img :src="require(`@/assets/weather/${getWeatherIcon(weatherData.hourly[2].weather[0].icon)}`)"
                  alt="Weather Icon">
                <p>{{ Math.round(weatherData.hourly[2].temp) }}°C</p>
                <p>{{ weatherData.hourly[2].weather[0].description }}</p>
              </div>

              <div class="forecast-hours">
                <p>{{ localTime(weatherData.hourly[3].dt, weatherData.timezone_offset) }}</p>
                <img :src="require(`@/assets/weather/${getWeatherIcon(weatherData.hourly[3].weather[0].icon)}`)"
                  alt="Weather Icon">
                <p>{{ Math.round(weatherData.hourly[3].temp) }}°C</p>
                <p>{{ weatherData.hourly[3].weather[0].description }}</p>
              </div>

              <div class="forecast-hours">
                <p>{{ localTime(weatherData.hourly[4].dt, weatherData.timezone_offset) }}</p>
                <img :src="require(`@/assets/weather/${getWeatherIcon(weatherData.hourly[4].weather[0].icon)}`)"
                  alt="Weather Icon">
                <p>{{ Math.round(weatherData.hourly[4].temp) }}°C</p>
                <p>{{ weatherData.hourly[4].weather[0].description }}</p>
              </div>

              <div class="forecast-hours">
                <p>{{ localTime(weatherData.hourly[5].dt, weatherData.timezone_offset) }}</p>
                <img :src="require(`@/assets/weather/${getWeatherIcon(weatherData.hourly[5].weather[0].icon)}`)"
                     alt="Weather Icon">
                <p>{{ Math.round(weatherData.hourly[5].temp) }}°C</p>
                <p>{{ weatherData.hourly[5].weather[0].description }}</p>
              </div>
            </div>
          </div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
export default {
  data() {
    return {
      api_key: 'df4a8b9b149ce7743751c9e1c3a1d189',
      url_base: 'https://api.openweathermap.org/data/2.5/onecall',
      url_location: 'https://api.openweathermap.org/geo/1.0/direct',
      query: '',
      weatherData: null,
      location: null,
      lat: '',
      lon: '',
    };
  },
  methods: {
    fetchLocation(e) {
      if (e.key == "Enter") {
        fetch(`${this.url_location}?q=${this.query}&appid=${this.api_key}`).then(res => {
          return res.json();
        }).then(this.locationResults);
      }
    },
    locationResults(resultsLocation) {
      this.location = resultsLocation;
      this.lat = this.location[0].lat
      this.lon = this.location[0].lon
      this.fetchWeather()
    },
    fetchWeather() {
      fetch(`${this.url_base}?lat=${this.lat}&lon=${this.lon}&units=metric&APPID=${this.api_key}`).then(res => {
        return res.json();
      }).then(this.weatherResults);
    },
    weatherResults(resultsWeather) {
      this.weatherData = resultsWeather;
    },
    getWeatherIcon(iconCode) {
      switch (iconCode) {
        case '01d':
          return `day-clear.svg`
          break;
        case '02d':
          return `day-cloudy.svg`
          break;
        case '03d' || '03n':
          return `cloud.svg`
          break;
        case '04d' || '04n':
          return `cloudy.svg`
          break;
        case '09d':
          return `day-shower.svg`
          break;
        case '10d':
          return `day-rain.svg`
          break;
        case '11d':
          return `day-thunderstorm.svg`
          break;
        case '13d':
          return `day-snow.svg`
          break;
        case '50d':
          return `day-fog.svg`
          break;
        case '01n':
          return `night-clear.svg`
          break;
        case '02n':
          return `night-cloudy.svg`
          break;
        case '09n':
          return `night-shower.svg`
          break;
        case '10n':
          return `night-rain.svg`
          break;
        case '11n':
          return `night-thunderstorm.svg`
          break;
        case '13n':
          return `night-snow.svg`
          break;
        case '50n':
          return `night-fog.svg`
          break;
        default:
          return `cloud.svg`
      }
    },
    getLocalDate(dt, timezone) {
      let dateTime = new Date(dt * 1000 + (timezone * 1000));

      let weekday = dateTime.toLocaleString('default', { weekday: 'long' });
      let month = dateTime.toLocaleString('default', { month: 'long' });
      let date = dateTime.getDate();

      return `${weekday}, ${date} ${month}`;
    },
    localTime(dt, timezone) {
      const utc_seconds = parseInt(dt, 10) + parseInt(timezone, 10);
      const utc_milliseconds = utc_seconds * 1000;
      const local_date = new Date(utc_milliseconds).toUTCString();

      const dateObject = new Date(local_date);
      const hours = dateObject.getUTCHours();
      const minutes = dateObject.getUTCMinutes();

      // Format hours and minutes as strings with leading zeros
      const formattedHours = hours.toString().padStart(2, '0');
      const formattedMinutes = minutes.toString().padStart(2, '0');

      // Combine hours and minutes into a formatted string
      const formattedTime = `${formattedHours}:${formattedMinutes}`;


      return `${formattedTime}`;
    }
  },
};
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Poppins:ital,wght@0,100;0,200;0,300;0,400;0,500;0,600;0,700;0,800;0,900;1,100;1,200;1,300;1,400;1,500;1,600;1,700;1,800;1,900&display=swap');

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: poppins, sans-serif;
}

main {
  background: linear-gradient(to right bottom, #3a3a3a, #1e1e1e);
  min-height: 100vh;
  width: 100%;
}

h1 {
  text-align: center;
  padding: 2rem 0;
  font-size: 3rem;
  background: linear-gradient(90deg, #06629b, #01dd96);
  background-clip: text;
  -webkit-text-fill-color: transparent;
}

.search-box {
  width: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
}

.search-bar {
  border-radius: 30px;
  width: 60%;
  height: 40px;
  background-color: #444444;
  color: #a6a6a6;
  font-size: 1rem;
  padding-left: 20px;
  border: none;
  margin: 1rem auto;
}

.cards {
  display: flex;
  justify-content: space-evenly;
  gap: 2rem;
  max-width: 1200px;
  margin: 2rem auto;
}

.cardsTwo {
  display: flex;
  justify-content: space-evenly;
  gap: 2rem;
  max-width: 1200px;
  margin: 0 auto;
  padding-bottom: 5rem;
}

.card {
  background: #444444;
  color: #ffffff;
  padding: 1rem;
  height: 18rem;
  border-radius: 30px;
  box-shadow: 8px 8px 4px 0 rgba(0, 0, 0, 0.5);
}

.card .location {
  text-align: center;
  font-weight: bold;
  padding-top: 1.5rem;
  font-size: 1.5rem;
}

.location {
  width: 30%;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
}

.location-card {
  width: 80%;
  text-align: center;
  font-weight: bold;
  padding-top: 1.5rem;
  font-size: 1.5rem;
}

.time {
  text-align: center;
  font-size: 4rem;
  font-weight: bolder;
  margin-top: 4rem;
}

.date {
  text-align: center;
  margin-top: -20px;
}

.weather {
  display: flex;
  justify-content: space-around;
  align-items: center;
  width: 65%;
}

.first-section h2 {
  font-size: 3.4rem;
  background: linear-gradient(90deg, #01dd96, #06629b);
  background-clip: text;
  -webkit-text-fill-color: transparent;
}

.first-section h3 {
  font-size: 1.2rem;
  margin-top: -20px;
  background: linear-gradient(270deg, #01dd96, #06629b);
  background-clip: text;
  -webkit-text-fill-color: transparent;
}

.temps {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
}

.sun {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
}

.sunrise {
  display: flex;
  justify-content: center;
  align-items: center;
  margin-top: 25px;
}

.sunrise img {
  height: 60px;
}

.sunrise h4 {
  font-size: 1rem;
  font-weight: normal;
}

.sunrise p {
  font-size: 0.8rem;
  margin-top: -5px;
}

.sunset {
  display: flex;
  justify-content: center;
  align-items: center;
}

.sunset img {
  height: 60px;
}

.sunset h4 {
  font-size: 1rem;
  font-weight: normal;
}

.sunset p {
  font-size: 0.8rem;
  margin-top: -5px;
}

.middle-section {
  display: flex;
  justify-content: space-evenly;
  align-items: center;
  flex-direction: column;
}

.middle-section img {
  height: 180px;
}

.middle-section p {
  margin: 1rem auto;
  font-size: 1.5rem;
  font-weight: bold;
  letter-spacing: 1px;
}

.last-section {
  display: grid;
  grid-template-columns: 50% 50%;
}

.last-section img {
  height: 75px;
  width: 100px;
}

.last-section p {
  font-size: 1rem;
  text-align: center;
}

.forecast-day {
  text-align: center;
  width: 30%;
}

.forecast-day h2 {
  margin-bottom: 20px;
}

.forecast-days {
  display: flex;
  justify-content: space-evenly;
  align-items: center;
  flex-direction: row;
}

.forecast-days img {
  height: 40px;
}

.forecast-hourly {
  width: 65%;
  text-align: center;
}

.forecast-card {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: row;
  gap: 1rem;
  padding: 1rem;

}

.forecast-hours {
  background: #373636;
  width: 20%;
  min-height: 211px;
  border-radius: 30px;
  padding: 1rem;
}

.forecast-hours img {
  height: 65px;
}

.forecast-hours p {
  font-weight: bold;
  text-align: center;
}

.forecast-hours p:first-child {
  font-size: 1.3rem;
}

.forecast-hours p:nth-child(3) {
  font-size: 1rem;
}

.forecast-hours p:nth-child(4) {
  font-size: 1rem;
  font-weight: normal;
}

@media (max-width: 641px) {
  .search-bar {
    width: 80%;
  }

  .cards {
    display: flex;
    gap: 2rem;
    flex-direction: column;
    max-width: 90%;
    margin: 2rem auto;
  }

  .cardsTwo {
    display: flex;
    justify-content: space-evenly;
    gap: 2rem;
    width: 90%;
    flex-direction: column;
  }

  .card {
    min-width: 95%;
    margin: 0 auto;
  }

  .weather {
    display: flex;
    justify-content: center;
    flex-direction: column;
    align-items: center;
    height: 90%;
  }

  .sun {
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: row;
  }

  .sunrise {
    margin: 15px;
    padding: 0 1rem;
  }

  .sunset {
    margin: 15px;
    padding: 0 1rem;
  }

  .first-section {
    flex-direction: row;
  }

  .middle-section img {
    height: 150px;
  }

  .middle-section p {
    margin: 0 auto;
  }

  .last-section {
    width: 100%;
  }

  .last-section img {
    width: 100%;
  }

  .forecast-card {
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: row;
    flex-wrap: wrap;
    gap: 1rem;
    padding: 1rem;
    height: 500px;
  }

  .forecast-hourly {
    width: 90%;
    height: 750px;
    margin-bottom: 3rem;
  }

  .forecast-hours {
    background: #373636;
    width: 45%;
    min-height: 211px;
    border-radius: 30px;
    padding: 1rem 0;
  }

  .forecast-hours img {
    height: 65px;
  }

  .forecast-hours p {
    font-weight: bold;
    text-align: center;
  }

  .forecast-hours p:first-child {
    font-size: 1.3rem;
  }

  .forecast-hours p:nth-child(3) {
    font-size: 1rem;
  }

  .forecast-hours p:nth-child(4) {
    font-size: 1rem;
    font-weight: normal;
  }
}</style>
