<template>
  <div class="city-not-found">
    <div class="container-city">
      <p class="para">{{ error_msg[select(lang)] }}</p>
    </div>
  </div>
</template>

<script>
export default {
  name: "CityNotFound",
  props: {
    lang: String
  },
  data() {
    return {
      error_msg: ["City not found", "Città inesistente"]
    };
  },
  methods: {
    select(lang) {
      if (lang === "IT") {
        return 0;
      } else {
        return 1;
      }
    }
  }
};
</script>

<style scoped>
.city-not-found {
  width: 200px;
  height: 50px;
  position: absolute;
  z-index: 200;
}

.para {
  font-weight: bold;
  margin: 0;
}

.container-city {
  position: absolute;
  background: #990000;
  opacity: 0.9;
  box-shadow: 0px 10px 25px 0px #990000;
  margin-top: 10px;
  padding: 10px 20px;
  border-radius: 6px 20px 20px 20px;
  color: antiquewhite;
  transition: all 0.3 ease;
}

@media only screen and (max-height: 410px) {
  .container-city {
    opacity: 1;
  }
}
</style>
