<template>
  <div class="toggle-mode fade_effect">
    <div class="wrap00" id="wrap00">
      <label for="check" class="con-switch">
        <input
          autocomplete="off"
          id="check"
          class="check"
          type="checkbox"
          :checked="mode === 'dark' ? 'checked' : false"
          @change="$emit('toggle')"
        />
        <div class="circle"></div>
      </label>
    </div>
  </div>
</template>

<script>
export default {
  name: "ToggleMode",
  props: ["mode"]
};
</script>

<style scoped>
.wrap00 {
  position: absolute;
  width: 50px;
  top: 20px;
  right: 10px;
}

.con-switch {
  width: 40px;
  background: rgba(99, 107, 144, 0.15);
  height: 20px;
  border-radius: 25px;
  cursor: pointer;
  border: 1px solid rgba(99, 107, 144, 0.25);
  display: flex;
}

.check {
  position: absolute;
  opacity: 0;
  cursor: pointer;
}

.circle {
  position: relative;
  left: 0px;
  top: 0px;
  margin: 0;
  width: 20px;
  height: 20px;
  border-radius: 20px;
  background: #990000;
  transition: all 0.25s ease;
  box-shadow: 0px 10px 20px 0px rgba(99, 107, 144, 0.35);
  opacity: 0.4;
}

.con-switch:hover {
  background: rgba(99, 107, 144, 0.25);
}

.check:checked ~ .circle {
  left: 20px;
  opacity: 1;
}

.con-switch:active .circle {
  width: -40px;
}

.con-switch:active .check:checked ~ .circle {
  width: 20px;
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
