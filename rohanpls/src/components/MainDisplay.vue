<template>
  <div class="hero-container" ref="container">
    <div class="background-image" :style="bgStyle"></div>
    
    <div class="content-wrapper">
      <h1>@rohanpls</h1>
       <Terminal />
    </div>
  </div>
</template>

<script setup>
import Terminal from './TheTerminal.vue';

import { ref, computed, onMounted, onUnmounted } from 'vue';

const container = ref(null);
const mouseX = ref(0);
const mouseY = ref(0);

const handleMouseMove = (event) => {
  if (!container.value) return;
  const rect = container.value.getBoundingClientRect();
  mouseX.value = event.clientX - (rect.left + rect.width / 2);
  mouseY.value = event.clientY - (rect.top + rect.height / 2);
};

const bgStyle = computed(() => {
  const parallaxStrength = 0.015;
  const x = mouseX.value * parallaxStrength;
  const y = mouseY.value * parallaxStrength;
  return {
    transform: `scale(1.1) translateX(${x}px) translateY(${y}px)`,
    backgroundImage: `url('/hero-background.jpg')`
  };
});

onMounted(() => {
  window.addEventListener('mousemove', handleMouseMove);
});
onUnmounted(() => {
  window.removeEventListener('mousemove', handleMouseMove);
});
</script>

<style scoped>
.hero-container {
  height: 100vh;
  width: 100%;
  position: relative;
  overflow: hidden;
  display: flex;
  justify-content: center;
  align-items: center;
}

.background-image {
  position: absolute;
  top: -5%;
  left: -5%;
  width: 110%;
  height: 110%;
  background-size: cover;
  background-position: center;
  transition: transform 0.2s ease-out;
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
  font-size: 5rem;
  font-weight: 800;
  color: white; 
}

@media (max-width: 768px) {
  h1 {
    font-size: 3rem;
  }
}
</style>