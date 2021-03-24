<template>
  <div class="search-bar fade_effect" :class="mode">
    <div class="container_input" :class="{ focused: focus }">
      <img :src="searchIcon" alt="Search icon" class="icon_search" />
      <form
        method="get"
        autocomplete="off"
        id="form"
        @submit.prevent="
          sendValue();
          sendInput();
          changeInput();
        "
      >
        <input
          :placeholder="msg(language)"
          type="text"
          class="form_input"
          name="searchbar"
          @focus="focus = true"
          @blur="focus = false"
          v-model="input"
        />
      </form>
      <button
        type="button"
        class="x-button"
        @click="deleteText"
        v-if="input.lenght > 0"
      >
        X
      </button>
      <img
        class="icon_pin"
        :src="pin"
        alt="pin"
        width="25"
        height="25"
        @click="$emit('locate')"
      />
    </div>
  </div>
</template>

<script>
export default {
  name: "SearchBar",
  data() {
    return {
      searchIcon: require("@/assets/Search.svg"),
      pin: require("@/assets/Pin-red.svg"),
      focus: false,
      text: ["Search city", "Cerca una città"],
      input: "",
      submit_input: false
    };
  },
  props: {
    mode: String,
    language: String
  },
  methods: {
    msg(language) {
      if (language === "IT") {
        return this.text[0];
      } else {
        return this.text[1];
      }
    },

    changeInput() {
      if (this.submit_input === false) {
        this.submit_input = true;
      } else {
        this.submit_input = false;
      }

      this.input = "";
    },

    sendValue() {
      this.$emit("return-value", this.input);
    },

    sendInput() {
      this.$emit("return-input", this.submit_input);
    },

    deleteText() {
      this.input = "";
    }
  }
};
</script>

<style scoped>
.search-bar {
  display: flex;
  justify-content: center;
  background: transparent;
}

.dark .search-bar {
  background: transparent;
}

.container_input {
  border-radius: 50px;
  display: flex;
  align-items: center;
  padding: 7.5px 15px;
  border-radius: 25px;
  border: none;
  box-shadow: 0px 8px 25px 0px rgba(0, 0, 0, 0.15);
  transition: all 0.3s ease;
  min-width: 250px;
  background-color: white;
}

form {
  min-width: 150px;
  width: 65vw;
  border: none;
}

.form_input {
  border: none;
  padding-left: -50px;
  min-width: 150px;
  width: 97%;
  font-family: rooney-sans, sans-serif;
  font-size: 15px;
  background-color: transparent;
  transition: all 0.3s ease;
  margin-left: 20px;
}

.form_input:focus {
  outline: none;
}

.form_input:focus ~ .icons {
  transform: translate(0, -6px);
}

.focused {
  transform: translate(0, -6px);
  box-shadow: 0px 15px 25px 0px rgba(0, 0, 0, 0.25);
  border: 0px;
  border-radius: 25px;
}

.dark .focused {
  transform: translate(0, -6px);
  box-shadow: 0px 15px 25px 0px rgba(255, 255, 255, 0.25);
  border: 0px;
  border-radius: 25px;
}

.icon_search {
  transition: all 0.25s ease;
  opacity: 0.7;
  max-width: 17px;
  max-height: 17px;
}

.icon_pin {
  transition: all 0.3s ease;
  opacity: 0.7;
  margin-left: 1vmin;
}

.icon_pin:hover {
  transition: all 0.25s ease;
  transform: translate(0, -6px);
  cursor: pointer;
  opacity: 1;
}

.x-button {
  position: absolute;
  right: 20vw;
  width: 20px;
  height: 20px;
  display: flex;
  align-items: center;
  justify-content: center;
  background-color: transparent;
  border: none;
  color: #414141;
  font-family: Nunito;
  font-weight: 800;
  font-style: Regular;
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
</style>
