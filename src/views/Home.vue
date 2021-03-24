<template>
  <div v-if="!animationCompleted" class="home" :class="mode">
    <logo-animation :isDark="isDark"></logo-animation>
  </div>
  <div v-if="animationCompleted" class="home" :class="mode">
    <toggle-mode :mode="mode" @toggle="toggle"></toggle-mode>
    <choose-language
      :lang="lang"
      mode="mode"
      @click="placeholder"
    ></choose-language>
    <home-page-logo
      :isDark="isDark"
      style="padding-top: 20vh;"
    ></home-page-logo>
    <div class="search_bar_container">
      <search-bar
        :mode="mode"
        :language="lang"
        @locate="
          showLocation();
          geo_notification_valid = true;
        "
        @return-value="getCity"
        @return-input="getInput"
      ></search-bar>
    </div>
    <div class="container-city">
      <validate-city
        v-if="city.length > 0"
        :city_input="city"
        :lang="lang"
        :submitted_again="submit_input"
        :page_info="current_page"
      ></validate-city>
    </div>
    <div
      class="geolocation-error"
      v-if="geo_notification && geo_notification_valid"
    >
      <geolocation-error :lang="lang"></geolocation-error>
    </div>
    <footer-item mode="mode"></footer-item>
  </div>
</template>

<script>
import ToggleMode from "@/components/ToggleMode.vue";
import ChooseLanguage from "@/components/ChooseLanguage.vue";
import FooterItem from "@/components/FooterItem.vue";
import HomePageLogo from "@/components/HomePageLogo.vue";
import SearchBar from "@/components/SearchBar.vue";
import LogoAnimation from "@/components/LogoAnimation.vue";
import GeolocationError from "@/components/GeolocationError.vue";
import ValidateCity from "@/components/ValidateCity.vue";

export default {
  name: "Home",
  components: {
    ToggleMode,
    ChooseLanguage,
    HomePageLogo,
    SearchBar,
    FooterItem,
    LogoAnimation,
    GeolocationError,
    ValidateCity
  },

  data() {
    return {
      mode: "light",
      isDark: false,
      animationCompleted: false,
      lang: localStorage.getItem("lang") || "IT",
      latitude: null,
      longitude: null,
      status: "off",
      geo_notification: false,
      geo_notification_valid: false,
      city: "",
      submit_input: "",
      isSubmitted: false,
      current_page: "home"
    };
  },

  methods: {
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

    getCity(value) {
      this.city = value;
    },

    getInput(value) {
      this.submit_input = value;
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

    loadingAnimation() {
      setTimeout(() => {
        this.animationCompleted = true;
      }, 2400);
    },

    placeholder() {
      if (this.lang === "IT") {
        this.lang = "ENG";
        localStorage.setItem("lang", "ENG");
      } else {
        this.lang = "IT";
        localStorage.setItem("lang", "IT");
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

    showLocation() {
      if (this.latitude === undefined && this.longitude === undefined) {
        this.geolocate();
      } else {
        localStorage.setItem("latitude_from_HP", this.latitude);
        localStorage.setItem("longitude_from_HP", this.longitude);
        this.$router.push({ path: "results" });
      }
    },

    clearStorage() {
      localStorage.removeItem("valid_city_from_HP");
      localStorage.removeItem("latitude_from_HP");
      localStorage.removeItem("longitude_from_HP");
      localStorage.removeItem("latitude_from_Results");
      localStorage.removeItem("longitude_from_Results");
    }
  },

  created() {
    this.geolocate();
    this.getStatus();
    this.clearStorage();
  },

  mounted() {
    this.loadingAnimation();
  },

  beforeUnmount() {
    localStorage.setItem("last_url", "Home");
  }
};
</script>

<style scoped>
.home {
  width: auto;
  height: 100vh;
  background-color: #e1e1e1;
  font-family: rooney-sans, sans-serif;
  font-weight: 400;
  font-style: normal;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  transition: background 0.3s ease-in-out;
}

.dark {
  background-color: #414141;
  transition: background 0.3s ease-in;
}

.search_bar_container {
  margin-top: 30px;
}

.geolocation-error {
  display: flex;
  justify-content: center;
  width: 80%;
  margin: 10px auto 0 auto;
}

.container-city {
  margin-left: 16%;
}

@media only screen and (min-width: 770px) {
  .geolocation-error {
    justify-content: right;
    margin-right: 18vw;
  }
}

@media only screen and (max-height: 410px) {
  .geolocation-error {
    margin-top: -20vh;
  }
}

@media only screen and (max-height: 440px) {
  .search_bar_container {
    margin-top: -5vh;
  }
}
</style>
