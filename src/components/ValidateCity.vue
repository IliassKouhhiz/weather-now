<template>
  <div class="validate-city" v-if="city_not_found">
    <city-not-found :lang="lang"></city-not-found>
  </div>
</template>

<script>
import axios from "axios";
import CityNotFound from "./CityNotFound.vue";

export default {
  name: "ValidateCity",
  components: { CityNotFound },
  data() {
    return {
      api_key: process.env.VUE_APP_API_KEY,
      api_data: null,
      city_not_found: false
    };
  },
  computed: {
    city: {
      get() {
        return this.city_input;
      },
      set(newValue) {
        this.city = newValue;
        return this.city;
      }
    }
  },
  props: {
    city_input: String,
    lang: String,
    page_info: String,
    submitted_again: Boolean
  },
  methods: {
    async fetch_data() {
      try {
        const response = await axios.get(
          `https://api.openweathermap.org/data/2.5/weather?q=${this.city}&appid=${this.api_key}`
        );
        this.api_data = await response;
        this.city_not_found = false;
        if (this.page_info === "home") {
          localStorage.setItem("valid_city_from_HP", this.city);
          this.$router.push({ path: "results" });
        }
      } catch (err) {
        console.log(err);
        this.cityNotFound();
      }

      this.sendCityStatus();
    },

    cityNotFound() {
      this.city_not_found = true;
      setTimeout(() => {
        this.city_not_found = false;
      }, 1500);
    },

    sendCityStatus() {
      this.$emit("city-status", this.city_not_found);
    }
  },

  watch: {
    submitted_again: function() {
      this.fetch_data();
    }
  },

  async mounted() {
    this.fetch_data();
  }
};
</script>

<style scoped></style>
