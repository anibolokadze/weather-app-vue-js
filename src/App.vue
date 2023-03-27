<template>
  <div
    id="app"
    :class="
      typeof weather.main != 'undefined' && weather.main.temp > 15 ? 'warm' : ''
    "
  >
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
          <div class="location">
            {{ weather.name }}, {{ weather.sys.country }}
          </div>
          <div class="date">{{ getDate() }}</div>

          <div class="weather-box">
            <div class="temp">{{ Math.round(weather.main.temp) }}°C</div>
            <div class="weather">{{ weather.weather[0].main }}</div>
          </div>
        </div>
      </div>

      <div v-if="weather.error">
        <p>{{ weather.error }}</p>
      </div>
    </main>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      api_key: "cccb7e39abfbe6a058cb1ca4edc0ca3e",
      url_base: "https://api.openweathermap.org/data/2.5/",
      query: "",
      weather: {},
    };
  },

  methods: {
    fetchWeather(e) {
      if (e.key == "Enter") {
        fetch(
          `${this.url_base}weather?q=${this.query}&units=metric&appid=${this.api_key}`
        )
          .then((res) => {
            if (!res.ok) {
              throw new Error("City not found");
            }
            return res.json();
          })
          .then(this.setResults)
          .catch(() => {
            this.weather = { error: "City not found" };
          });
      }
    },
    setResults(results) {
      this.weather = results;
    },
    getDate() {
      let currentDate = new Date();
      let weekDay = currentDate.toLocaleDateString("en-US", {
        weekday: "long",
      });
      let date = currentDate.getDate();
      const month = currentDate.toLocaleString("default", { month: "long" });
      let year = currentDate.getFullYear();
      return `${weekDay} ${date} ${month} ${year}`;
    },
  },
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
p {
  font-weight: 700;
  text-align: center;
}
#app {
  background-image: url("./assets/cold-bg.jpg");
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}
#app.warm {
  background-image: url("./assets/warm-bg.jpg");
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
  margin-top: 100px;
}
.search-box .search-bar {
  display: block;
  width: 80%;
  padding: 15px;
  margin: 0 auto;
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
}
.search-box .search-bar:focus {
  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.75);
  border-radius: 16px 0px 16px 0px;
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
  font-style: italic;
  text-align: center;
}
.weather-box {
  text-align: center;
}
.weather-box .temp {
  display: inline-block;
  padding: 10px 25px;
  color: #fff;
  font-size: 102px;
  font-weight: 900;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.25);
  border-radius: 16px;
  margin: 30px 0px;
  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
.weather-box .weather {
  color: #fff;
  font-size: 48px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
</style>
