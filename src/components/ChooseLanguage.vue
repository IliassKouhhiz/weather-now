<template>
  <div class="choose-language fade_effect" :class="mode">
    <div class="lang" id="lang">
      <p class="lang-par">{{ lang }}</p>
    </div>
  </div>
</template>

<script>
export default {
  name: "ChooseLanguage",
  props: {
    lang: String,
    mode: String
  }
};
</script>

<style scoped>
.lang {
  position: fixed;
  width: 60px;
  height: 30px;
  top: 60px;
  right: 0px;
  background-color: #414141c0;
  display: flex;
  border-radius: 20px 0px 0px 20px;
  cursor: pointer;
  transition: background-color 0.3s ease-in-out;
}

.dark .lang {
  position: fixed;
  width: 60px;
  height: 30px;
  top: 60px;
  right: 0px;
  background-color: #e1e1e1c0;
  display: flex;
  border-radius: 20px 0px 0px 20px;
  cursor: pointer;
  transition: background-color 0.3s ease-in-out;
}

.lang:hover {
  transform: scale(1.2);
  background-color: #414141;
  transition: all 0.25s ease-in-out;
}

.dark .lang:hover {
  transform: scale(1.2);
  background-color: #e1e1e1;
  transition: all 0.25s ease-in-out;
}

.lang-par {
  font-size: 1.1em;
  margin-left: 10px;
  color: #e1e1e1;
}
.dark .lang-par {
  color: #414141;
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
