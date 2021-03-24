<template>
  <div class="data-animation">
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
  name: "DataAanimation",
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
.data-animation {
  height: 100vh;
}

@keyframes animation {
  from {
    opacity: 0.4;
    transform: scale(0.85);
    margin-top: 35vh;
  }

  50% {
    opacity: 0.7;
    transform: scale(0.95);
    margin-top: 35vh;
  }

  to {
    opacity: 0.4;
    transform: scale(0.85);
    margin-top: 35vh;
  }
}

.logo {
  display: flex;
  justify-content: center;
}

.logo_light {
  animation-name: animation;
  animation-duration: 2.5s;
  animation-iteration-count: 4;
  justify-content: center;
  transform: scale(0.85);
  opacity: 0.7;
  max-width: 526.32px;
  max-height: 149.81px;
  transition: all 0.25s ease-in;
}

.logo_dark {
  animation-name: animation;
  animation-duration: 2.5s;
  animation-iteration-count: 4;
  justify-content: center;
  transform: scale(0.85);
  max-width: 526.32px;
  max-height: 149.81px;
  transition: all 0.25s ease-in;
}

@media only screen and (min-width: 600px) and (min-height: 600px) {
  @keyframes animation {
    from {
      opacity: 0.4;
      transform: scale(1);
      margin-top: 35vh;
    }

    50% {
      opacity: 0.7;
      transform: scale(1.1);
      margin-top: 35vh;
    }

    to {
      opacity: 0.4;
      transform: scale(1);
      margin-top: 35vh;
    }
  }

  .logo_light {
    transform: scale(1);
  }

  .logo_dark {
    transform: scale(1);
  }
}
</style>
