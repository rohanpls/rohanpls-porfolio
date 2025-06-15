<template>
  <div class="main-display-container">
    <div class="background-image" :style="bgStyle"></div>
    <div class="content-wrapper">
      <h1>@rohanpls</h1>
      <TheTerminal v-if="isTerminalVisible" />
    </div>
  </div>
</template>

<script setup>
import { computed } from 'vue'
import TheTerminal from './TheTerminal.vue'

// Define the prop that App.vue will pass down
defineProps({
  isTerminalVisible: {
    type: Boolean,
    default: true,
  },
})

const bgStyle = computed(() => ({
  backgroundImage: `url('/hero-background.jpg')`,
}))
</script>

<style lang="scss" scoped>
.main-display-container {
  height: 100vh;
  width: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
  position: relative;
  overflow: hidden;
}
.background-image {
  position: absolute;
  top: -5%;
  left: -5%;
  width: 110%;
  height: 110%;
  background-size: cover;
  background-position: center;
  z-index: 0;
}
.content-wrapper {
  position: relative;
  z-index: 1;
  display: flex;
  flex-direction: column;
  align-items: center;
}
h1 {
  font-family: 'Montserrat', sans-serif;
  font-size: 6rem;
  font-weight: 700;
  color: white;
  margin: 0;
  text-shadow: 0 4px 20px rgba(0, 0, 0, 0.4);
}
/* We add a transition to the terminal for smooth appearing/disappearing */
:deep(.terminal-window) {
  transition:
    opacity 0.3s ease,
    transform 0.3s ease;
}
</style>
