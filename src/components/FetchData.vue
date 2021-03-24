<template>
  <div class="fetch-data"></div>
</template>

<script>
import axios from "axios";

export default {
  components: {},

  data() {
    return {
      api_key: process.env.VUE_APP_API_KEY,
      api_data_1: null,
      api_data_2: null,
      data_received: false
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
    submitted_again: Boolean,
    pin_again: Boolean
  },
  methods: {
    async cityFromResults() {
      if (this.city.length > 0 && this.lang === "IT") {
        try {
          const response_1 = await axios.get(
            `https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=${this.api_key}`
          );
          this.api_data_1 = await response_1;
          this.city_not_found = false;

          let lat = this.api_data_1.data.coord.lat;
          let long = this.api_data_1.data.coord.lon;

          const response_2 = await axios.get(
            `https://api.openweathermap.org/data/2.5/onecall?lat=${lat}&lon=${long}&units=metric&appid=${this.api_key}`
          );
          this.api_data_2 = await response_2;

          this.data_received = true;
        } catch (err) {
          console.log(err);
          this.data_received = false;
        }
      } else if (this.city.length > 0 && this.lang === "ENG") {
        try {
          const response_1 = await axios.get(
            `https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&lang=it&appid=${this.api_key}`
          );
          this.api_data_1 = await response_1;
          this.city_not_found = false;

          let lat = this.api_data_1.data.coord.lat;
          let long = this.api_data_1.data.coord.lon;

          const response_2 = await axios.get(
            `https://api.openweathermap.org/data/2.5/onecall?lat=${lat}&lon=${long}&units=metric&lang=it&appid=${this.api_key}`
          );
          this.api_data_2 = await response_2;

          this.data_received = true;
        } catch (err) {
          console.log(err);
          this.data_received = false;
        }
      }
    },

    async cityFromHP() {
      if (
        localStorage.getItem("valid_city_from_HP") !== null &&
        this.lang === "IT"
      ) {
        const response_1 = await axios.get(
          `https://api.openweathermap.org/data/2.5/weather?q=${localStorage.getItem(
            "valid_city_from_HP"
          )}&units=metric&appid=${this.api_key}`
        );
        this.api_data_1 = await response_1;

        let lat = this.api_data_1.data.coord.lat;
        let long = this.api_data_1.data.coord.lon;

        const response_2 = await axios.get(
          `https://api.openweathermap.org/data/2.5/onecall?lat=${lat}&lon=${long}&units=metric&appid=${this.api_key}`
        );
        this.api_data_2 = await response_2;

        this.data_received = true;
      } else if (
        localStorage.getItem("valid_city_from_HP") !== null &&
        this.lang === "ENG"
      ) {
        const response_1 = await axios.get(
          `https://api.openweathermap.org/data/2.5/weather?q=${localStorage.getItem(
            "valid_city_from_HP"
          )}&units=metric&lang=it&appid=${this.api_key}`
        );
        this.api_data_1 = await response_1;

        let lat = this.api_data_1.data.coord.lat;
        let long = this.api_data_1.data.coord.lon;

        const response_2 = await axios.get(
          `https://api.openweathermap.org/data/2.5/onecall?lat=${lat}&lon=${long}&units=metric&lang=it&appid=${this.api_key}`
        );
        this.api_data_2 = await response_2;

        this.data_received = true;
      }
    },

    async coordsFromHP() {
      if (
        this.lang === "IT" &&
        localStorage.getItem("latitude_from_HP") !== null
      ) {
        try {
          const response_1 = await axios.get(
            `https://api.openweathermap.org/data/2.5/weather?lat=${localStorage.getItem(
              "latitude_from_HP"
            )}&lon=${localStorage.getItem(
              "longitude_from_HP"
            )}&units=metric&appid=${this.api_key}`
          );
          this.api_data_1 = await response_1;

          const response_2 = await axios.get(
            `https://api.openweathermap.org/data/2.5/onecall?lat=${localStorage.getItem(
              "latitude_from_HP"
            )}&lon=${localStorage.getItem(
              "longitude_from_HP"
            )}&units=metric&appid=${this.api_key}`
          );
          this.api_data_2 = await response_2;

          this.data_received = true;
        } catch (err) {
          console.log(err);
          this.data_received = false;
        }
      } else if (
        this.lang === "ENG" &&
        localStorage.getItem("latitude_from_HP") !== null
      ) {
        try {
          const response_1 = await axios.get(
            `https://api.openweathermap.org/data/2.5/weather?lat=${localStorage.getItem(
              "latitude_from_HP"
            )}&lon=${localStorage.getItem(
              "longitude_from_HP"
            )}&units=metric&lang=it&appid=${this.api_key}`
          );
          this.api_data_1 = await response_1;

          const response_2 = await axios.get(
            `https://api.openweathermap.org/data/2.5/onecall?lat=${localStorage.getItem(
              "latitude_from_HP"
            )}&lon=${localStorage.getItem(
              "longitude_from_HP"
            )}&units=metric&lang=it&appid=${this.api_key}`
          );
          this.api_data_2 = await response_2;
          this.data_received = true;
        } catch (err) {
          console.log(err);
          this.data_received = false;
        }
      }
    },

    async coordsFromResults() {
      if (
        localStorage.getItem("latitude_from_Results") !== null &&
        this.lang === "IT"
      ) {
        try {
          const response_1 = await axios.get(
            `https://api.openweathermap.org/data/2.5/weather?lat=${localStorage.getItem(
              "latitude_from_Results"
            )}&lon=${localStorage.getItem(
              "longitude_from_Results"
            )}&units=metric&appid=${this.api_key}`
          );
          this.api_data_1 = await response_1;

          const response_2 = await axios.get(
            `https://api.openweathermap.org/data/2.5/onecall?lat=${localStorage.getItem(
              "latitude_from_Results"
            )}&lon=${localStorage.getItem(
              "longitude_from_Results"
            )}&units=metric&appid=${this.api_key}`
          );
          this.api_data_2 = await response_2;

          this.data_received = true;
        } catch (err) {
          console.log(err);
          this.data_received = false;
        }
      } else if (
        this.lang &&
        localStorage.getItem("latitude_from_Results") !== null
      ) {
        try {
          const response_1 = await axios.get(
            `https://api.openweathermap.org/data/2.5/weather?lat=${localStorage.getItem(
              "latitude_from_Results"
            )}&lon=${localStorage.getItem(
              "longitude_from_Results"
            )}&units=metric&lang=it&appid=${this.api_key}`
          );
          this.api_data_1 = await response_1;

          const response_2 = await axios.get(
            `https://api.openweathermap.org/data/2.5/onecall?lat=${localStorage.getItem(
              "latitude_from_Results"
            )}&lon=${localStorage.getItem(
              "longitude_from_Results"
            )}&units=metric&lang=it&appid=${this.api_key}`
          );
          this.api_data_2 = await response_2;

          this.data_received = true;
        } catch (err) {
          console.log(err);
          this.data_received = false;
        }
      }
    },

    sendFetchStatus() {
      this.$emit("return-data-received", this.data_received);
    },

    sendFetchData_1() {
      this.$emit("send-data-1", this.api_data_1);
    },

    sendFetchData_2() {
      this.$emit("send-data-2", this.api_data_2);
    }
  },

  watch: {
    submitted_again: function() {
      this.cityFromResults();
    },

    pin_again: function() {
      this.coordsFromResults();
    },

    data_received: function() {
      this.sendFetchStatus();
    },

    api_data_1: function() {
      this.sendFetchData_1();
    },

    api_data_2: function() {
      this.sendFetchData_2();
    }
  },

  async mounted() {
    this.cityFromResults();
    this.cityFromHP();
    this.coordsFromHP();
    this.coordsFromResults();
  }
};
</script>

<style scoped></style>
