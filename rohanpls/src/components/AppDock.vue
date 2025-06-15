<template>
  <div class="dock-container">
    <nav class="app-dock">
      <div class="dock-section weather-display"></div>

      <div class="dock-section app-icons">
        <div v-for="app in apps" :key="app.name" class="dock-item" @click="launchApp(app.name)">
          <div class="icon-wrapper">
            <div class="icon-svg" v-html="app.icon"></div>
          </div>
          <span class="tooltip">{{ app.title }}</span>
        </div>
      </div>

      <div class="dock-section time-display">
        <div class="time">{{ currentTime }}</div>
        <div class="date">{{ currentDate }}</div>
      </div>
    </nav>
  </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

defineProps({
  apps: { type: Array, required: true },
})

const emit = defineEmits(['launch-app'])

const launchApp = (appName) => {
  emit('launch-app', appName)
}

// --- Reactive State for Time ---
const currentTime = ref('')
const currentDate = ref('')
let timeInterval = null

// --- Function to update time and date ---
const updateTimeAndDate = () => {
  const now = new Date()
  currentTime.value = now.toLocaleTimeString('en-US', {
    hour: '2-digit',
    minute: '2-digit',
    hour12: false,
  })
  currentDate.value = now.toLocaleDateString('en-US', {
    weekday: 'short',
    month: 'short',
    day: 'numeric',
  })
}

// --- Lifecycle Hooks ---
onMounted(() => {
  updateTimeAndDate()
  timeInterval = setInterval(updateTimeAndDate, 1000)
  // The fetchWeather() call has been removed.
})

onUnmounted(() => {
  clearInterval(timeInterval)
})
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
  justify-content: space-between;
  align-items: center;
  width: 60%;
  max-width: 1920px;
  padding: 12px 20px;
  background-color: rgba(40, 45, 60, 0.65);
  backdrop-filter: blur(10px);
  -webkit-backdrop-filter: blur(10px);
  border-radius: 50px;
  border: 1px solid rgba(255, 255, 255, 0.1);
  box-shadow: 0 8px 32px rgba(0, 0, 0, 0.3);
  pointer-events: auto;
  font-family: 'Inter', sans-serif;
  color: #e0e0e0;
}

.dock-section {
  display: flex;
  align-items: center;
  gap: 15px;
  flex: 1;
}

.app-icons {
  justify-content: center;
}

/* The weather-display class is kept for layout purposes but is empty */
.weather-display {
  justify-content: flex-start;
}

.time-display {
  justify-content: flex-end;
  text-align: right;
  margin-right: 15px;
}
.time {
  font-size: 1.2rem;
  font-weight: 500;
}
.date {
  font-size: 0.8rem;
  opacity: 0.7;
}

/* All other styles for icons and tooltips remain the same */
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
