<template>
  <div class="dock-container">
    <div class="app-dock">
      <div v-for="app in apps" :key="app.name" class="dock-item" @click="launchApp(app.name)">
        <div class="icon-wrapper">
          <div class="icon-svg" v-html="app.icon"></div>
        </div>
        <span class="tooltip">{{ app.title }}</span>
      </div>
    </div>
  </div>
</template>

<script setup>
defineProps({
  apps: { type: Array, required: true },
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
  z-index: 2000;
  pointer-events: none;
}

.app-dock {
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 15px;
  width: 80%;
  max-width: 500px;
  padding: 12px;
  background-color: rgba(40, 45, 60, 0.65);
  backdrop-filter: blur(10px);
  -webkit-backdrop-filter: blur(10px);
  border-radius: 50px;
  border: 1px solid rgba(255, 255, 255, 0.1);
  box-shadow: 0 8px 32px rgba(0, 0, 0, 0.3);
  pointer-events: auto;
}

.dock-item {
  position: relative;
  cursor: pointer;
  padding: 5px;
}

.icon-wrapper {
  width: 48px;
  height: 48px;
  display: flex;
  justify-content: center;
  align-items: center;
  transition: transform 0.3s cubic-bezier(0.175, 0.885, 0.32, 1.275);
}

.dock-item:hover .icon-wrapper {
  transform: translateY(-15px) scale(1.1);
}

.icon-svg {
  width: 28px;
  height: 28px;
  color: #aeb3b8;
  transition: color 0.3s ease;
}

.dock-item:hover .icon-svg {
  color: white;
}

.icon-svg :deep(svg) {
  width: 100%;
  height: 100%;
  stroke: currentColor;
}

.tooltip {
  position: absolute;
  bottom: 120%;
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
  transform: translateX(-50%) translateY(-5px);
}
</style>
