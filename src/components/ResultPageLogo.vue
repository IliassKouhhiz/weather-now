<template>
  <div class="result-page-logo">
    <img
      v-if="isDark === true && device_width > 550"
      :src="logo_dark"
      alt="Weather Now Logo Dark"
      class="logo"
    />
    <img
      v-else-if="isDark === false && device_width > 550"
      :src="logo_light"
      alt="Weather Now Logo"
      class="logo"
    />
    <img
      v-else-if="isDark === true"
      :src="logo_dark_min"
      alt="Weather Now Logo"
      class="logo_min"
    />
    <img v-else :src="logo_light_min" alt="Weather Now Logo" class="logo_min" />
  </div>
</template>

<script>
export default {
  name: "ResultPageLogo",
  data() {
    return {
      logo_light: require("@/assets/weatherNow-logo.svg"),
      logo_dark: require("@/assets/weatherNow-logo-dark.svg"),
      logo_light_min: require("@/assets/weatherNow-logo-min.svg"),
      logo_dark_min: require("@/assets/weatherNow-logo-light-min.svg"),
      device_width: window.innerWidth
    };
  },

  props: {
    isDark: Boolean
  },

  methods: {},

  mounted() {
    window.addEventListener("resize", () => {
      this.device_width = window.innerWidth;
    });
  }
};
</script>

<style scoped>
.result-page-logo {
  cursor: pointer;
}

.logo {
  opacity: 0.7;
  width: 150px;
  height: 60px;
  margin: 10px auto auto 20px;
}

.logo_min {
  opacity: 0.7;
  width: 52px;
  height: 42.25px;
  margin: 10px auto auto 20px;
}

.logo_dark {
  transform: scale(0.85);
  max-width: 526.32px;
  max-height: 149.81px;
}
</style>
