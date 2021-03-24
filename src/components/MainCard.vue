<template>
  <div class="main-card" :class="mode">
    <span id="data1" v-show="side === 'front'" @click="changeSide">
      <p class="today-name">{{ name }}</p>
      <p v-if="language === 'IT'" class="today-date" id="today-date">
        {{ date }}
      </p>
      <p v-if="language === 'ENG'" class="today-date" id="today-date">
        {{ date_ita }}
      </p>
      <div id="icon"><i id="today-icon"></i></div>
      <p class="today-temperature" id="today-temperature">
        {{ temperature }}° C
      </p>
      <p v-if="language === 'IT'" class="today-feel" id="today-feel">
        Feels like {{ feel }}.
      </p>
      <p v-if="language === 'ENG'" class="today-feel" id="today-feel">
        Si percepiscono {{ feel_ita }}.
      </p>
    </span>
    <span id="data2" v-show="side === 'back'" @click="changeSide">
      <div class="pair" id="sunrise">
        <i class="wi wi-sunrise cc"></i>
        <p v-if="language === 'IT'" class="text" id="sunrise-text">
          <strong>Sunrise:</strong> {{ sunrise }}
        </p>
        <p v-if="language === 'ENG'" class="text" id="sunrise-text">
          <strong>Alba:</strong> {{ sunrise }}
        </p>
      </div>
      <div class="pair" id="sunset">
        <i class="wi wi-sunset cc"></i>
        <p v-if="language === 'IT'" class="text" id="sunset-text">
          <strong>Sunset:</strong> {{ sunset }}
        </p>
        <p v-if="language === 'ENG'" class="text" id="sunset-text">
          <strong>Tramonto:</strong> {{ sunset }}
        </p>
      </div>
      <div class="pair" id="high-low">
        <i class="wi wi-thermometer cc"></i>
        <p class="text" id="high-low-text">
          <strong>Max:</strong>{{ high }}, <strong>Min:</strong>{{ low }}
        </p>
      </div>
      <div class="pair" id="humidity">
        <i class="wi wi-humidity cc"></i>
        <p v-if="language === 'IT'" class="text" id="humidity-text">
          <strong>Humidity: </strong>{{ humidity }}%
        </p>
        <p v-if="language === 'ENG'" class="text" id="humidity-text">
          <strong>Umidità: </strong>{{ humidity }}%
        </p>
      </div>
      <div class="pair" id="uv">
        <i class="wi wi-hot cc"></i>
        <p class="text" id="uv-text"><strong>UV:</strong> {{ uv }}</p>
      </div>
      <div class="pair" id="wind">
        <i class="wi wi-strong-wind cc"></i>
        <p v-if="language === 'IT'" class="text" id="wind-text">
          <strong>Wind: </strong>{{ wind }}
        </p>
        <p v-if="language === 'ENG'" class="text" id="wind-text">
          <strong>Vento: </strong>{{ wind }}
        </p>
      </div>
      <div class="pair" id="visiblity">
        <i class="wi wi-windy cc"></i>
        <p v-if="language === 'IT'" class="text" id="visibility-text">
          <strong>Visibility:</strong> {{ visibility }}
        </p>
        <p v-if="language === 'ENG'" class="text" id="visibility-text">
          <strong>Visibilità:</strong> {{ visibility }}
        </p>
      </div>
    </span>
    <div class="back-drop"></div>
  </div>
</template>

<script>
import moment from "moment-timezone";
import axios from "axios";
import "bootstrap";

require("@/assets/styles/weather-icons-wind.css");
require("@/assets/styles/weather-icons-wind.min.css");
require("@/assets/styles/weather-icons.css");
require("@/assets/styles/weather-icons.min.css");

export default {
  name: "MainCardTest",
  props: {
    today_data_1: Object,
    today_data_2: Object,
    mode: String,
    language: String
  },

  data() {
    return {
      data_1: this.today_data_1,
      data_2: this.today_data_2,
      card: true,
      icons: null,
      side: "front"
    };
  },

  computed: {
    name: {
      get() {
        if (this.data_1.data.name.indexOf("Province") === 0) {
          return `${this.data_1.data.name.slice(12)}, ${
            this.data_1.data.sys.country
          }`;
        } else if (this.data_1.data.name.indexOf("Comune") === 0) {
          return `${this.data_1.data.name.slice(10)}, ${
            this.data_1.data.sys.country
          }`;
        } else {
          return `${this.data_1.data.name}, ${this.data_1.data.sys.country}`;
        }
      }
    },
    date: {
      get() {
        return this.moment(this.data_2.data.current.dt).format(
          "ddd D MMM YYYY, HH:mm "
        );
      }
    },
    date_ita: {
      get() {
        return `${this.translateDay(
          this.moment(this.data_2.data.current.dt).format("ddd")
        )} 
        ${this.moment(this.data_2.data.current.dt).format("D")} 
        ${this.translateMonth(
          this.moment(this.data_2.data.current.dt).format("MMM")
        )} 
        ${this.moment(this.data_2.data.current.dt).format("YYYY, HH:mm")}`;
      }
    },
    temperature: {
      get() {
        return this.data_1.data.main.temp.toFixed();
      }
    },
    feel: {
      get() {
        return `${this.data_1.data.main.feels_like.toFixed()}°C. ${
          this.data_1.data.weather[0].main
        }, ${this.data_1.data.weather[0].description}`;
      }
    },
    feel_ita: {
      get() {
        return `${this.data_1.data.main.feels_like.toFixed()}°C. ${this.translateMain(
          this.data_1.data.weather[0].main
        )}, ${this.data_1.data.weather[0].description}`;
      }
    },
    humidity: {
      get() {
        return this.data_2.data.current.humidity;
      }
    },
    sunrise: {
      get() {
        return this.moment(this.data_1.data.sys.sunrise).format("HH:mm");
      }
    },
    uv: {
      get() {
        return this.data_2.data.daily[0].uvi.toFixed();
      }
    },
    sunset: {
      get() {
        return this.moment(this.data_1.data.sys.sunset).format("HH:mm");
      }
    },
    wind: {
      get() {
        return `${this.data_2.data.current.wind_speed}m/s, ${this.windDirection(
          this.data_2.data.current.wind_deg
        )}`;
      }
    },
    high: {
      get() {
        return ` ${this.data_1.data.main.temp_max.toFixed()}°C`;
      }
    },
    low: {
      get() {
        return ` ${this.data_1.data.main.temp_min.toFixed()}°C`;
      }
    },
    visibility: {
      get() {
        return `${(this.data_2.data.current.visibility * 0.001).toFixed()}km`;
      }
    }
  },

  methods: {
    moment(code) {
      let snippet = moment.unix(code).tz(this.data_2.data.timezone);
      return snippet;
    },

    translateMain(input) {
      let output;

      if (input === "Clouds") {
        output = "Nuvoloso";
      } else if (input === "Thunderstorm") {
        output = "Temporale";
      } else if (input === "Drizzle") {
        output = "Pioggerella";
      } else if (input === "Rain") {
        output = "Pioggia";
      } else if (input === "Snow") {
        output = "Neve";
      } else if (input === "Mist") {
        output = "Nebbiolina";
      } else if (input === "Smoke") {
        output = "Fumo";
      } else if (input === "Haze") {
        output = "Foschia";
      } else if (input === "Dust") {
        output = "Pulviscolo";
      } else if (input === "Fog") {
        output = "Nebbia";
      } else if (input === "Sand") {
        output = "Sabbia";
      } else if (input === "Ash") {
        output = "Cenere";
      } else if (input === "Squall") {
        output = "Burrasca";
      } else if (input === "Tornado") {
        output = "Tornado";
      } else if (input === "Clear") {
        output = "Sereno";
      }

      return output;
    },

    translateDay(input) {
      let giorno;

      if (input === "Mon") {
        giorno = "Lun";
      } else if (input === "Tue") {
        giorno = "Mar";
      } else if (input === "Wed") {
        giorno = "Mer";
      } else if (input === "Thu") {
        giorno = "Gio";
      } else if (input === "Fri") {
        giorno = "Ven";
      } else if (input === "Sat") {
        giorno = "Sab";
      } else if (input === "Sun") {
        giorno = "Dom";
      }

      return giorno;
    },

    translateMonth(input) {
      let mese;

      if (input === "Jan") {
        mese = "Gen";
      } else if (input === "May") {
        mese = "Mag";
      } else if (input === "Jun") {
        mese = "Giu";
      } else if (input === "Jul") {
        mese = "Lug";
      } else if (input === "Aug") {
        mese = "Ago";
      } else if (input === "Sep" || input === "Sept") {
        mese = "Set";
      } else if (input === "Oct") {
        mese = "Ott";
      } else if (input === "Dec") {
        mese = "Dic";
      } else {
        mese = input;
      }

      return mese;
    },

    windDirection(degree) {
      var sectors = ["N", "NE", "E", "SE", "S", "SW", "W", "NW"];
      degree += 22.5;
      if (degree < 0) degree = 360 - (Math.abs(degree) % 360);
      else degree = degree % 360;
      var which = parseInt(degree / 45);
      return sectors[which];
    },

    async weatherIcons() {
      const url =
        "https://gist.githubusercontent.com/tbranyen/62d974681dea8ee0caa1/raw/3405bfb2a76b7cbd90fde33d8536f0cd13706955/icons.json";
      const response_1 = await axios.get(url);
      this.icons = await response_1;
      let icons = await response_1;
      let data1 = this.data_1;
      let data2 = this.data_2;
      let momentLB = this.moment;

      function getIconCode(code) {
        var icon = icons.data[code].icon;
        var label = icons.data[code].label;

        function setId(icon) {
          const date = momentLB(data2.data.current.dt).format("HH:mm");
          const sunrise = momentLB(data1.data.sys.sunrise).format("HH:mm");
          const sunset = momentLB(data1.data.sys.sunset).format("HH:mm");
          let weatherIconID = "";

          if (date >= sunrise && date < sunset) {
            weatherIconID = `wi wi-day-${icon}`;
            if (icon == "day-haze") {
              weatherIconID = `wi wi-${icon}`;
            }
            if (icon == "smoke") {
              weatherIconID = `wi wi-${icon}`;
            }
            if (label == "scattered clouds") {
              weatherIconID = `wi wi-cloud`;
            }
            if (label == "broken clouds" || label == "overcast clouds") {
              weatherIconID = `wi wi-cloudy`;
            }
            if (label == "light rain") {
              weatherIconID = `wi wi-day-sleet`;
            }
            if (label == "moderate rain") {
              weatherIconID = `wi wi-day-rain-wind`;
            }
            if (label == "dust") {
              weatherIconID = `wi wi-dust`;
            }
            if (label == "smoke") {
              weatherIconID = `wi wi-smoke`;
            }
            if (icon == "sprinkle") {
              weatherIconID = `wi wi-fog`;
            }
          } else {
            weatherIconID = `wi wi-night-${icon}`;
            if (icon == "sunny") {
              weatherIconID = "wi wi-night-clear";
            }
            if (icon == "day-haze") {
              weatherIconID = "wi wi-night-fog";
            }
            if (label == "scattered clouds") {
              weatherIconID = `wi wi-cloud`;
            }
            if (label == "broken clouds" || label == "overcast clouds") {
              weatherIconID = `wi wi-cloudy`;
            }
            if (label == "light rain") {
              weatherIconID = `wi wi-night-sleet`;
            }
            if (label == "moderate rain") {
              weatherIconID = `wi wi-night-rain-wind`;
            }
            if (label == "dust") {
              weatherIconID = `wi wi-dust`;
            }
            if (label == "smoke") {
              weatherIconID = `wi wi-smoke`;
            }
            if (icon == "sprinkle") {
              weatherIconID = `wi wi-fog`;
            }
          }
          return weatherIconID;
        }
        let weatherIconID = setId(icon);
        return weatherIconID;
      }

      let todayIcon = document.getElementById("today-icon");
      todayIcon.setAttribute(
        "class",
        getIconCode(this.data_1.data.weather[0].id)
      );
      todayIcon.setAttribute("alt", "Weather icon");
    },

    // Change the colour of the card according to the weather
    async cardColour() {
      let colours = {
        cloudsDay: "linear-gradient(#fffff3, #4fc3f7)",
        cloudsNight: "linear-gradient(#4286f4, #373B44)",
        clearDay: "linear-gradient(#ffff1c, #4fc3f7)",
        clearNight: "linear-gradient(#F8CDDA, #1D2B64)",
        thunder: "linear-gradient(#16222A, #3A6073)",
        rain: "linear-gradient(#274046, #E6DADA)",
        snow: "linear-gradient(#eef2f3, #8e9eab)",
        fog: "linear-gradient(#C9D6FF, #E2E2E2)",
        sand: "linear-gradient(#f8b500, #fceabb)"
      };
      let weather_description;
      let data1 = this.data_1;
      let data2 = this.data_2;
      let momentLB = this.moment;

      function getN(code) {
        const date = momentLB(data2.data.current.dt).format("HH:mm");
        const sunrise = momentLB(data1.data.sys.sunrise).format("HH:mm");
        const sunset = momentLB(data1.data.sys.sunset).format("HH:mm");

        if (date >= sunrise && date < sunset) {
          if (code == 800 || code == 904 || code == 771) {
            weather_description = "clearDay";
          }
          if (code >= 200 && code < 300) {
            weather_description = "thunder";
          }
          if (code >= 900 && code <= 902) {
            weather_description = "thunder";
          }
          if (code > 800 && code < 900) {
            weather_description = "cloudsDay";
          }
          if (code > 904 && code < 960) {
            weather_description = "cloudsDay";
          }
          if (code >= 300 && code < 600) {
            weather_description = "rain";
          }
          if (code >= 600 && code < 700) {
            weather_description = "snow";
          }
          if (code >= 700 && code <= 741) {
            weather_description = "fog";
          }
          if (code >= 751 && code <= 762) {
            weather_description = "sand";
          }
          if (code >= 960) {
            weather_description = "thunder";
          }
        } else {
          if (code == 800) {
            weather_description = "clearNight";
          }
          if (code > 800 && code < 900) {
            weather_description = "cloudsNight";
          }
          if (code > 904 && code < 960) {
            weather_description = "cloudsNight";
          }
          if (code >= 300 && code < 600) {
            weather_description = "rain";
          }
          if (code >= 600 && code < 700) {
            weather_description = "snow";
          }
          if (code >= 700 && code <= 741) {
            weather_description = "fog";
          }
          if (code >= 751 && code <= 762) {
            weather_description = "sand";
          }
          if (code >= 200 && code < 300) {
            weather_description = "thunder";
          }
          if (code >= 900 && code <= 902) {
            weather_description = "thunder";
          }
          if (code >= 960) {
            weather_description = "thunder";
          }
        }

        return colours[weather_description];
      }

      document.querySelector(".main-card").style["background"] = getN(
        data1.data.weather[0].id
      );
    },

    changeSide() {
      if (this.side === "front") {
        this.side = "back";
      } else {
        this.side = "front";
      }
    },

    uploadedData() {
      setTimeout(() => {
        this.cardColour();
        this.weatherIcons();
      }, 500);
    }
  },

  async mounted() {
    this.weatherIcons(), this.cardColour();
  },

  watch: {
    today_data_1: function() {
      this.data_1 = this.today_data_1;
      this.uploadedData();
    },
    today_data_2: function() {
      this.data_2 = this.today_data_2;
    },
    side: function() {
      this.weatherIcons();
      this.cardColour();
    }
  }
};
</script>

<style scoped>
.main-card {
  border-radius: 40px;
  width: 100%;
  min-width: 320px;
  max-width: 400px;
  min-height: 400px;
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 20;
  border: none;
  cursor: pointer;
}

.light {
  box-shadow: inset 0px 0px 60px 20px rgba(225, 225, 225, 0.65);
  transition: box-shadow 0.3s ease-in-out;
}
.light:hover {
  transform: scale(1.05);
  box-shadow: 0px 15px 25px 0px rgba(0, 0, 0, 0.25);
  transition: all 0.3s ease-in-out;
}

.dark {
  box-shadow: inset 0px 0px 60px 20px rgb(65, 65, 65, 0.8);
  transition: box-shadow 0.3s ease-in-out;
}
.dark:hover {
  transform: scale(1.05);
  box-shadow: inset 0px 0px 15px 15px rgb(65, 65, 65, 0.8);
  box-shadow: 0px 15px 25px 0px rgba(255, 255, 255, 0.25);
  transition: all 0.3s ease-in-out;
}

@keyframes card-animation {
  from {
    opacity: 0;
    transform: scale(0.9);
  }
  to {
    opacity: 1;
    transform: scale(1);
  }
}

#data1 {
  display: grid;
  grid-template-columns: 1fr;
  row-gap: 2vh;
  column-gap: 60px;
  width: 100%;
  height: 100%;
  animation-name: card-animation;
  animation-duration: 1s;
  animation-iteration-count: 1;
  transition: background 0.3s ease-in-out;
}

.today-name {
  font-size: 38px;
  font-weight: 600;
  margin: 15px auto;
  text-align: center;
}

#icon {
  height: 120px;
  width: 120px;
  margin: -40px auto 0px auto;
  display: flex;
  justify-content: center;
  align-items: center;
}

#icon > i {
  transform: scale(7);
}

.today-date {
  font-weight: 500;
  height: 20px;
  font-size: 25px;
  margin-top: -50px;
  text-align: center;
}

.today-temperature {
  font-size: 5em;
  font-weight: 600;
  width: 100%;
  text-align: center;
}

.today-feel {
  font-size: 18px;
  font-size: 22px;
  margin: -20px auto 0px auto;
  text-align: center;
  width: 90%;
}

#data2 {
  display: grid;
  grid-template-columns: 1fr;
  row-gap: 1em;
  column-gap: 60px;
  width: 100%;
  height: 100%;
  padding: 30px 0px;
  animation-name: card-animation;
  animation-duration: 1s;
  animation-iteration-count: 1;
}

.cards {
  width: 100%;
  max-width: 100%;
  border-radius: inherit;
  z-index: 2;
}

.shadow-1 {
  width: 98vw;
  height: 75vh;
  background: #151a29;
  content: "";
  position: absolute;
  border-radius: inherit;
  opacity: 0.2;
  filter: blur(10px);
  z-index: -1;
  left: 2.5px;
}

.shadow-1-dark {
  width: 95%;
  height: 95%;
  background: #ffffff;
  content: "";
  position: absolute;
  border-radius: inherit;
  opacity: 0.2;
  filter: blur(20px);
  z-index: -1;
  left: 2.5px;
}

.shadow-2 {
  width: 95%;
  height: 95%;
  content: "";
  position: absolute;
  border-radius: inherit;
  opacity: 1;
  filter: blur(10px);
  z-index: -1;
  left: 5px;
}

.pair {
  display: flex;
  flex-wrap: wrap;
  align-items: center;
  padding-left: 15%;
}

.cc {
  transform: scale(1.6);
}

.text {
  position: absolute;
  padding-left: 50px;
  font-size: 20px;
}

p {
  margin: 0;
  line-height: 1.3;
}

/*  MEDIA QUERIES  */
@media only screen and (min-width: 1000px) {
  #data2 {
    min-width: 400px;
    min-height: 400px;
  }

  #icon {
    height: 120px;
    width: 120px;
    margin: -20px auto 0px auto;
    display: flex;
    justify-content: center;
    align-items: center;
  }

  #icon > i {
    transform: scale(6.5);
  }

  .today-date {
    font-weight: 500;
    height: 20px;
    font-size: 25px;
    margin-top: -40px;
    text-align: center;
  }

  .today-temperature {
    font-size: 4.5em;
    font-weight: 600;
    width: 100%;
    text-align: center;
  }

  .today-feel {
    font-size: 18px;
    font-size: 22px;
    margin: -20px auto 0px auto;
    text-align: center;
    width: 90%;
  }
}
</style>
