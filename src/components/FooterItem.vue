<template>
  <div class="footer-item fade_effect" :class="mode">
    <a
      v-for="social in socials"
      :href="social.url"
      target="_blank"
      :key="social.index"
      :id="social.name"
    >
      <img :src="social.path" :alt="social.name" />
      <div v-if="social.name === 'e-mail' && is_active === true" class="email">
        <p>{{ social.tag }}</p>
      </div>
    </a>
  </div>
</template>

<script>
export default {
  name: "FooterItem",
  data() {
    return {
      socials: [
        {
          path: require("@/assets/e-mail.svg"),
          name: "e-mail",
          url: "//",
          tag: "iliass.kouhhiz@gmail.com"
        },
        {
          path: require("@/assets/linkedin.svg"),
          name: "linkedin",
          url: "https://it.linkedin.com/in/iliass-kouhhiz"
        },
        {
          path: require("@/assets/github.svg"),
          name: "github",
          url: "https://github.com/IliassKouhhiz"
        },
        {
          path: require("@/assets/behance.svg"),
          name: "behance",
          url: "https://www.behance.net/iliasskouhdd8b/moodboards"
        }
      ],
      is_active: false
    };
  },

  props: {
    mode: String
  },

  methods: {
    showTag() {
      let btn = document.getElementById("e-mail");
      btn.addEventListener("click", () => {
        if (this.is_active === true) {
          this.is_active = false;
        } else {
          this.is_active = true;
        }
      });
    }
  },

  mounted() {
    this.showTag();
  }
};
</script>

<style scoped>
.footer-item {
  background-color: #414141;
  width: 100%;
  height: 40px;
  display: flex;
  justify-content: center;
  align-items: center;
  position: fixed;
  bottom: 0%;
  transition: background-color 0.3s ease-in-out;
  border-radius: 20px 20px 0px 0px;
}

.dark .footer-item {
  background-color: #e1e1e1;
  width: 100%;
  height: 40px;
  display: flex;
  justify-content: center;
  align-items: center;
  position: fixed;
  bottom: 0%;
  transition: background-color 0.3s ease-in-out;
}

.footer-item a {
  margin-left: 2%;
  margin-right: 2%;
}

.footer-item a img:hover {
  transition: all 0.3s ease-out;
  transform: translate(0, -10px);
}

img {
  width: 31.5px;
  height: 31.5px;
}

.fade_effect {
  animation: fade-in 1s;
  animation-delay: 0s;
}

@keyframes fade-in {
  from {
    opacity: 0;
  }

  to {
    opacity: 1;
  }
}

.email {
  position: fixed;
  left: 31vw;
  bottom: 50px;
  z-index: 10;
  border-radius: 15px 15px 15px 0px;
  background-color: #990000;
  transition: all 0.3s ease-in-out;
  animation: fade-in 0.5s;
  animation-delay: 0s;
  cursor: default;
}

.email > p {
  padding: 10px;
  margin: 0px;
  color: white;
}

/*  MEDIA QUERIES  */
@media only screen and (min-width: 700px) {
  .email {
    position: fixed;
    left: 40vw;
    bottom: 50px;
  }
}

@media only screen and (min-width: 400px) and (max-width: 700px) {
  .email {
    position: fixed;
    left: 35vw;
    bottom: 50px;
  }
}
</style>
