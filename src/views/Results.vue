<template>
  <div
    v-if="!data_received && city_status === false"
    class="results"
    :class="mode"
  >
    <data-animation :isDark="isDark"></data-animation>
  </div>
  <div
    v-if="data_received || city_status === true"
    class="results"
    :class="mode"
  >
    <header class="header">
      <div class="logo">
        <result-page-logo
          :isDark="isDark"
          @click="this.$router.push({ path: '/' })"
        ></result-page-logo>
      </div>
      <div v-if="device_width > 550" class="search_bar_container_1">
        <search-bar
          :mode="mode"
          :language="lang"
          @return-value="getCity"
          @return-input="getInput"
          @locate="
            showLocation();
            geo_notification_valid = true;
          "
        ></search-bar>
      </div>
      <div class="toggle">
        <toggle-mode :mode="mode" @toggle="toggle"></toggle-mode>
      </div>
    </header>
    <div v-if="device_width > 550" class="container-city-xl">
      <validate-city
        v-if="city.length > 0"
        :city_input="city"
        :lang="lang"
        :submitted_again="submit_input"
        :page_info="current_page"
        @city-status="getCityStatus"
      ></validate-city>
    </div>
    <div v-if="device_width <= 550" class="search_bar_container">
      <search-bar
        :mode="mode"
        :language="lang"
        @return-value="getCity"
        @return-input="getInput"
        @locate="
          showLocation();
          geo_notification_valid = true;
        "
      ></search-bar>
    </div>
    <div v-if="device_width <= 550" class="container-city">
      <validate-city
        v-if="city.length > 0"
        :city_input="city"
        :lang="lang"
        :submitted_again="submit_input"
        :page_info="current_page"
        @city-status="getCityStatus"
      ></validate-city>
    </div>
    <div
      class="geolocation-error"
      v-if="geo_notification && geo_notification_valid"
    >
      <geolocation-error :lang="lang"></geolocation-error>
    </div>
    <div class="results-cards">
      <div class="main">
        <main-card
          :mode="mode"
          :language="lang"
          :today_data_1="data_1"
          :today_data_2="data_2"
          :page="current_page"
        ></main-card>
      </div>
      <ul class="forecast-grid">
        <li>
          <forecast-card
            :mode="mode"
            :language="lang"
            :today_data_1="data_1"
            :today_data_2="data_2"
            :page="current_page"
            :day_of_the_week="1"
          ></forecast-card>
        </li>
        <li>
          <forecast-card
            :mode="mode"
            :language="lang"
            :today_data_1="data_1"
            :today_data_2="data_2"
            :page="current_page"
            :day_of_the_week="2"
          ></forecast-card>
        </li>
        <li>
          <forecast-card
            :mode="mode"
            :language="lang"
            :today_data_1="data_1"
            :today_data_2="data_2"
            :page="current_page"
            :day_of_the_week="3"
          ></forecast-card>
        </li>
        <li>
          <forecast-card
            :mode="mode"
            :language="lang"
            :today_data_1="data_1"
            :today_data_2="data_2"
            :page="current_page"
            :day_of_the_week="4"
          ></forecast-card>
        </li>
        <li>
          <forecast-card
            :mode="mode"
            :language="lang"
            :today_data_1="data_1"
            :today_data_2="data_2"
            :page="current_page"
            :day_of_the_week="5"
          ></forecast-card>
        </li>
        <li>
          <forecast-card
            :mode="mode"
            :language="lang"
            :today_data_1="data_1"
            :today_data_2="data_2"
            :page="current_page"
            :day_of_the_week="6"
          ></forecast-card>
        </li>
      </ul>
    </div>
  </div>
  <fetch-data
    :city_input="city"
    :lang="lang"
    @return-data-received="getFetchStatus"
    :submitted_again="submit_input"
    :pin_again="pin_input"
    @send-data-1="getData_1"
    @send-data-2="getData_2"
  ></fetch-data>
</template>

<script>
import SearchBar from "@/components/SearchBar.vue";
import ToggleMode from "@/components/ToggleMode.vue";
import ResultPageLogo from "@/components/ResultPageLogo.vue";
import GeolocationError from "@/components/GeolocationError.vue";
import FetchData from "@/components/FetchData.vue";
import DataAnimation from "@/components/DataAnimation.vue";
import ValidateCity from "@/components/ValidateCity.vue";
import MainCard from "../components/MainCard.vue";
import ForecastCard from "../components/ForecastCard.vue";

export default {
  name: "Results",
  components: {
    SearchBar,
    ToggleMode,
    ResultPageLogo,
    GeolocationError,
    FetchData,
    DataAnimation,
    ValidateCity,
    MainCard,
    ForecastCard
  },

  data() {
    return {
      mode: "light",
      isDark: false,
      status: "off",
      data_received: false,
      city: "",
      city_status: false,
      submit_input: "",
      pin_input: "",
      latitude: null,
      longitude: null,
      geo_notification: false,
      geo_notification_valid: false,
      device_width: window.innerWidth,
      lang: localStorage.getItem("lang") || "IT",
      lasturl: localStorage.getItem("last_url"),
      data_1: null,
      data_2: null,
      current_page: "results"
    };
  },

  methods: {
    refresh() {
      if (this.lasturl === "Results") {
        this.$router.push({ path: "/" });
      }
    },

    toggle() {
      if (this.mode === "dark") {
        this.mode = "light";
        this.isDark = false;
        this.status = "on";
      } else {
        this.mode = "dark";
        this.isDark = true;
        this.status = "off";
      }
      localStorage.setItem("status", this.status);
    },

    getStatus() {
      let get_status = localStorage.getItem("status");

      if (get_status == "on") {
        this.mode = "light";
        this.isDark = false;
      } else {
        this.mode = "dark";
        this.isDark = true;
      }
    },

    geolocate() {
      if (navigator.geolocation) {
        navigator.geolocation.getCurrentPosition(
          position => {
            this.latitude = position.coords.latitude;
            this.longitude = position.coords.longitude;
          },
          error => {
            console.log(error.message);
            this.latitude = undefined;
            this.longitude = undefined;
            this.geo_notification = true;
            setTimeout(() => {
              this.geo_notification = false;
            }, 3000);
          }
        );
      }
    },

    getCity(value) {
      this.city = value;
    },

    getInput(value) {
      this.submit_input = value;
    },

    getCityStatus(value) {
      this.city_status = value;
    },

    getData_1(value) {
      this.data_1 = value;
    },

    getData_2(value) {
      this.data_2 = value;
    },

    getFetchStatus(value) {
      this.data_received = value;
    },

    pinAgain() {
      if (this.pin_input === false) {
        this.pin_input = true;
      } else {
        this.pin_input = false;
      }
    },

    showLocation() {
      if (this.latitude === undefined && this.longitude === undefined) {
        this.geolocate();
      } else {
        localStorage.setItem("latitude_from_Results", this.latitude);
        localStorage.setItem("longitude_from_Results", this.longitude);
        this.pinAgain();
      }
    }
  },

  watch: {
    data_2: function() {
      console.log(this.data_1);
      console.log(this.data_2);
    }
  },

  created() {
    this.refresh();
  },

  mounted() {
    window.addEventListener("resize", () => {
      this.device_width = window.innerWidth;
    });
    this.getStatus();
    this.geolocate();
  },

  beforeUpdate() {
    localStorage.setItem("last_url", "Results");
  }
};
</script>

<style scoped>
.results {
  width: auto;
  height: 100%;
  min-height: 100vh;
  background-color: #e1e1e1;
  font-family: rooney-sans, sans-serif;
  font-weight: 400;
  font-style: normal;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  transition: background 0.3s ease-in-out;
  background-size: cover;
  background-position: center-bottom;
  background-repeat: no-repeat;
}

.dark {
  background-color: #414141;
  transition: background 0.3s ease-in;
}

.header {
  display: flex;
  flex-wrap: nowrap;
  height: 60px;
  width: 100%;
}

.search_bar_container {
  margin: 10px auto 0 auto;
  height: 40px;
}

.search_bar_container_1 {
  position: relative;
  margin: 20px 50px;
  height: 40px;
  width: 50%;
  flex: 1;
}

.logo {
  width: 150px;
  flex: initial;
}

.toggle {
  width: 150px;
  flex: initial;
}

.geolocation-error {
  display: flex;
  justify-content: center;
  width: 80%;
  margin: 10px auto 0 auto;
  opacity: 1;
  position: relative;
}

.container-city {
  margin-left: 16%;
}

.container-city-xl {
  margin-left: 230px;
}

.main {
  margin-top: 30px;
  display: flex;
  flex-wrap: wrap;
  justify-content: space-around;
  align-content: space-around;
}

.forecast-grid {
  width: 100%;
  padding-bottom: 40px;
  margin: 40px auto 0px auto;
  display: grid;
  grid-template-rows: 325px 325px 325px 325px 325px 325px;
  grid-template-columns: 100%;
  row-gap: 40px;
  column-gap: 15px;
  list-style-type: none;
}

li {
  justify-content: center;
  display: flex;
  width: inherit;
}

/*  MEDIA QUERIES  */
@media only screen and (min-width: 1300px) {
  .container-city-xl {
    margin-left: 16%;
  }

  .results-cards {
    display: flex;
    justify-content: space-around;
    align-items: center;
    margin: 45px auto;
  }

  .forecast-grid {
    width: 810px;
    padding-bottom: 0px;
    margin: 0px;
    display: grid;
    grid-template-rows: 240px 240px;
    grid-template-columns: 250px 250px 250px;
    row-gap: 20px;
    column-gap: 30px;
    list-style-type: none;
    justify-content: space-around;
    align-items: center;
  }

  li {
    width: 250px;
  }

  .main {
    margin-top: 0px;
  }
}

@media only screen and (min-width: 1000px) and (max-width: 1299px) {
  .search_bar_container_1 {
    width: 70%;
  }

  .geolocation-error {
    justify-content: right;
    margin: 10px -15vw 0 0;
  }

  .forecast-grid {
    width: 810px;
    height: 100%;
    padding-bottom: 30px;
    margin: 30px auto 0px auto;
    display: grid;
    grid-template-rows: 240px 240px;
    grid-template-columns: 250px 250px 250px;
    row-gap: 20px;
    column-gap: 30px;
    list-style-type: none;
    justify-content: space-around;
    align-items: center;
  }

  li {
    width: 250px;
  }
}

@media only screen and (min-width: 700px) and (max-width: 999px) {
  .search_bar_container_1 {
    width: 60%;
  }

  .forecast-grid {
    width: 100%;
    padding-bottom: 40px;
    margin: 40px auto 0px auto;
    display: grid;
    grid-template-rows: 270px 270px 270px;
    grid-template-columns: 325px 325px;
    row-gap: 40px;
    column-gap: 15px;
    list-style-type: none;
    justify-content: center;
  }
}

@media only screen and (min-width: 551px) and (max-width: 649px) {
  .geolocation-error {
    justify-content: right;
    margin-right: 20vw;
    width: 50%;
  }
}

@media only screen and (min-width: 650px) and (max-width: 999px) {
  .geolocation-error {
    justify-content: right;
    margin-right: 25vw;
    width: 50%;
  }
}

@media only screen and (max-height: 410px) {
  .geolocation-error {
    margin-top: -20vh;
  }
}
</style>
