<template>
  <div class="container-err">
    <p>
      <strong>{{ title[select(lang)] }}</strong>
    </p>
    <p>{{ error_msg_1[select(lang)] }}</p>
    <p>{{ error_msg_2[select(lang)] }}</p>
  </div>
</template>

<script>
export default {
  name: "GeolocationError",
  props: {
    lang: String
  },
  data() {
    return {
      title: ["Geolocation Error:", "Errore Geolocalizzazione:"],
      error_msg_1: [
        "1. Geolocation needs to be enabled in order to use this feature.",
        "1. E' necessario abilitare la geolocalizzazione se si vuole utilizzare questa funzione."
      ],
      error_msg_2: [
        "2. Your browser doesn't support this feature.",
        "2. Il browser utilizzato non supporta la geolocalizzazione."
      ]
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
.container-err {
  background: #990000;
  opacity: 0.8;
  box-shadow: 0px 10px 25px 0px #990000;
  padding: 10px 20px;
  border-radius: 20px 6px 20px 20px;
  color: antiquewhite;
  transition: all 0.3 ease;
  max-width: 500px;
  z-index: 2;
}

p {
  margin-bottom: 8px;
}

@media only screen and (max-height: 410px) {
  .container-err {
    opacity: 1;
    z-index: 2;
    border-radius: 20px;
  }
}
</style>
