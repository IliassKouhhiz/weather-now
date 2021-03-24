<template>
  <div class="home-page-logo">
    <div class="logo">
      <img
        v-if="isDark === true"
        :src="logo_dark"
        alt="Weather Now Logo Dark"
        class="logo_dark"
      />
      <img v-else :src="logo_light" alt="Weather Now Logo" class="logo_light" />
    </div>
  </div>
</template>

<script>
export default {
  name: "HomePageLogo",
  data() {
    return {
      logo_light: require("@/assets/weatherNow-logo.svg"),
      logo_dark: require("@/assets/weatherNow-logo-dark.svg")
    };
  },
  props: {
    isDark: Boolean
  }
};
</script>

<style scoped>
.logo {
  display: flex;
  justify-content: center;
}

.logo_light {
  transform: scale(0.85);
  opacity: 0.7;
  max-width: 526.32px;
  max-height: 149.81px;
}

.logo_dark {
  transform: scale(0.85);
  max-width: 526.32px;
  max-height: 149.81px;
}

@media only screen and (max-height: 440px) {
  .home-page-logo {
    margin-top: -10vh;
    transform: scale(0.85);
  }
}

@media only screen and (min-width: 600px) {
  .logo_light,
  .logo_dark {
    transform: scale(1);
  }
}
</style>
