<template>
  <div
    class="window-wrapper"
    :style="{ top: position.y + 'px', left: position.x + 'px', zIndex }"
    @mousedown="bringToFront"
  >
    <div class="title-bar" @mousedown.prevent="startDrag">
      <div class="title">{{ title }}</div>
      <div class="buttons">
        <div class="btn close" @click="$emit('close')"></div>

        <div class="btn minimize" @click="$emit('close')"></div>

        <div class="btn maximize"></div>
      </div>
    </div>
    <div class="window-body">
      <slot></slot>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const props = defineProps({
  title: { type: String, default: 'App' },
  initialPosition: { type: Object, default: () => ({ x: 150, y: 150 }) },
  zIndex: { type: Number, default: 10 },
})

const emit = defineEmits(['close', 'focus'])

const position = ref({ ...props.initialPosition })

const startDrag = (event) => {
  bringToFront()
  const initialMouseX = event.clientX
  const initialMouseY = event.clientY
  const initialWinX = position.value.x
  const initialWinY = position.value.y

  const handleMouseMove = (moveEvent) => {
    position.value.x = initialWinX + (moveEvent.clientX - initialMouseX)
    position.value.y = initialWinY + (moveEvent.clientY - initialMouseY)
  }

  const handleMouseUp = () => {
    document.removeEventListener('mousemove', handleMouseMove)
    document.removeEventListener('mouseup', handleMouseUp)
  }

  document.addEventListener('mousemove', handleMouseMove)
  document.addEventListener('mouseup', handleMouseUp)
}

const bringToFront = () => {
  emit('focus')
}
</script>

<style scoped>
.window-wrapper {
  position: fixed;
  width: 800px;
  height: 500px;
  border-radius: 12px;
  border: 1px solid #4a4a4a;
  box-shadow: 0 20px 50px rgba(0, 0, 0, 0.5);
  display: flex;
  flex-direction: column;
}

.title-bar {
  background-color: #3a3d4a;
  padding: 10px 12px;
  height: 56px;
  border-top-left-radius: 11px;
  border-top-right-radius: 11px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  cursor: grab;
}
.title-bar:active {
  cursor: grabbing;
}

.buttons {
  display: flex;
  gap: 8px;
}
.btn {
  width: 12px;
  height: 12px;
  border-radius: 50%;
}
.close {
  background-color: #ff5f56;
  cursor: pointer;
}
.minimize {
  background-color: #ffbd2e;
}
.maximize {
  background-color: #27c93f;
}

.title {
  color: #ccc;
  font-family: sans-serif;
  font-size: 0.8rem;
}

.window-body {
  flex-grow: 1;
  padding: 10px;
  background-color: rgba(15, 15, 15, 0.5);
  backdrop-filter: blur(20px);
  border-bottom-left-radius: 11px;
  border-bottom-right-radius: 11px;
}
</style>
