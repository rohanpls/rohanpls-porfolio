<template>
  <div class="dock-container">
    <div class="app-dock">
      <div v-for="app in apps" :key="app.name" class="dock-item" @click="launchApp(app.name)">
        <div class="icon-wrapper">
          <span class="icon-initials">{{ app.initials }}</span>
        </div>
        <span class="tooltip">{{ app.title }}</span>
      </div>
    </div>
  </div>
</template>

<script setup>
defineProps({
  apps: {
    type: Array,
    required: true,
  },
})

const emit = defineEmits(['launch-app'])

const launchApp = (appName) => {
  emit('launch-app', appName)
}
</script>

<style scoped>
.dock-container {
  position: fixed;
  bottom: 20px;
  left: 0;
  width: 100%;
  display: flex;
  justify-content: center;
  z-index: 2000; /* Ensure it's on top of other UI */
  pointer-events: none; /* The container itself isn't clickable */
}

.app-dock {
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 15px;
  width: 80%;
  max-width: 500px; /* Max width so it doesn't get too big on large screens */
  padding: 12px;

  /* Glassmorphism effect */
  background-color: rgba(40, 45, 60, 0.65);
  backdrop-filter: blur(10px);
  -webkit-backdrop-filter: blur(10px);

  /* Pill shape and border */
  border-radius: 50px;
  border: 1px solid rgba(255, 255, 255, 0.1);
  box-shadow: 0 8px 32px rgba(0, 0, 0, 0.3);

  pointer-events: auto; /* Enable clicks on the dock itself */
}

.dock-item {
  position: relative;
  cursor: pointer;
}

.icon-wrapper {
  width: 50px;
  height: 50px;
  background-color: rgba(255, 255, 255, 0.05);
  border-radius: 50%; /* Make icons circular */
  display: flex;
  justify-content: center;
  align-items: center;
  transition: all 0.3s cubic-bezier(0.175, 0.885, 0.32, 1.275);
}

.dock-item:hover .icon-wrapper {
  transform: translateY(-15px) scale(1.1);
  background-color: rgba(255, 255, 255, 0.15);
}

.icon-initials {
  font-family: 'Montserrat', sans-serif;
  font-weight: 600;
  font-size: 1.2rem;
  color: white;
}

.tooltip {
  position: absolute;
  bottom: 140%; /* Position above the icon */
  left: 50%;
  transform: translateX(-50%);
  background-color: #1e1e1e;
  color: white;
  padding: 5px 12px;
  border-radius: 5px;
  white-space: nowrap;
  opacity: 0;
  pointer-events: none;
  transition: all 0.3s cubic-bezier(0.175, 0.885, 0.32, 1.275);
  transform-origin: bottom center;
}

.dock-item:hover .tooltip {
  opacity: 1;
  transform: translateX(-50%) translateY(-10px);
}
</style>
