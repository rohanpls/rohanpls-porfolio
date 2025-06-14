<template>
  <div class="terminal-window" @click="focusInput">
    <div class="title-bar">
      <div class="tabs-container">
        <div
          v-for="tab in tabs"
          :key="tab.id"
          class="tab-item"
          :class="{ active: tab.isActive }"
          @click="selectTab(tab.id)"
        >
          <span>zsh</span>
          <div class="close-tab-btn" @click.stop="closeTab(tab.id)">×</div>
        </div>
        <div class="new-tab-btn" @click="addTab" v-if="tabs.length < maxTabs">+</div>
      </div>

      <div class="buttons">
        <div class="btn close"></div>
        <div class="btn minimize"></div>
        <div class="btn maximize"></div>
      </div>
    </div>

    <div class="terminal-body" ref="terminalBody">
      <template v-if="currentTab">
        <div v-for="(line, index) in currentTab.history" :key="index" class="line">
          <span v-if="line.type === 'command'" class="prompt">{{ prompt }}</span>
          <span>{{ line.text }}</span>
        </div>
        <div class="input-line">
          <span class="prompt">{{ prompt }}</span>
          <input
            ref="inputField"
            type="text"
            v-model="currentTab.input"
            @keydown.enter="handleCommand"
            @keydown.tab.prevent
            autofocus
          />
        </div>
      </template>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, watch, nextTick, onMounted, onUnmounted, inject } from 'vue'
const openApp = inject('openApp')

const tabs = ref([])
const prompt = 'rohanpls@portfolio:~$'
const inputField = ref(null)
const terminalBody = ref(null)

const isMobile = ref(false)

const checkScreenSize = () => {
  isMobile.value = window.innerWidth <= 768
}

const maxTabs = computed(() => {
  return isMobile.value ? 2 : 3
})

onMounted(() => {
  checkScreenSize()
  window.addEventListener('resize', checkScreenSize)
  addTab()
})

onUnmounted(() => {
  window.removeEventListener('resize', checkScreenSize)
})

const currentTab = computed(() => tabs.value.find((tab) => tab.isActive))

const createNewTab = () => ({
  id: Date.now(),
  history: [{ type: 'output', text: "Welcome! Type 'help' to see commands." }],
  input: '',
  isActive: false,
})

const addTab = () => {
  if (tabs.value.length >= maxTabs.value) {
    return
  }
  tabs.value.forEach((t) => (t.isActive = false))
  const newTab = createNewTab()
  newTab.isActive = true
  tabs.value.push(newTab)
}

const selectTab = (id) => {
  tabs.value.forEach((t) => (t.isActive = t.id === id))
}

const closeTab = (id) => {
  const index = tabs.value.findIndex((t) => t.id === id)
  if (index === -1) return

  const wasActive = tabs.value[index].isActive
  tabs.value.splice(index, 1)

  if (wasActive && tabs.value.length > 0) {
    const newActiveIndex = Math.max(0, index - 1)
    tabs.value[newActiveIndex].isActive = true
  } else if (tabs.value.length === 0) {
    addTab()
  }
}

const commands = {
  help: () => [
    "'whoami'",
    "'projects'",
    "'skills'",
    "'tictactoe'",
    "'contact'",
    "'date'",
    "'clear'",
  ],
  whoami: () => ['Rohan, a passionate developer.'],
  projects: () => {
    openApp('projects')
    return 'Launching Projects...'
  },
  skills: () => {
    openApp('skills')
    return 'Loading Skill Visualizer...'
  },
  tictactoe: () => {
    openApp('tictactoe')
    return 'Launching Tic Tac Toe...'
  },
  contact: () => ['Email: dev.rohanpls@gmail.com', 'LinkedIn: linkedin.com/in/rohanpls'],
  date: () => new Date().toLocaleString(),
  clear: () => {
    if (currentTab.value) currentTab.value.history = []
    return []
  },
}

const handleCommand = () => {
  if (!currentTab.value) return
  const tab = currentTab.value
  const command = tab.input.trim().toLowerCase()

  tab.history.push({ type: 'command', text: command })

  if (command in commands) {
    const output = commands[command]()
    if (Array.isArray(output)) tab.history.push(...output.map((text) => ({ type: 'output', text })))
    else tab.history.push({ type: 'output', text: output })
  } else if (command !== '') {
    tab.history.push({ type: 'output', text: `zsh: command not found: ${command}` })
  }
  tab.input = ''
}

const focusInput = () => inputField.value?.focus()

watch(
  () => currentTab.value?.history,
  async () => {
    await nextTick()
    if (terminalBody.value) terminalBody.value.scrollTop = terminalBody.value.scrollHeight
  },
  { deep: true },
)
</script>

<style scoped>
.terminal-window {
  width: 700px;
  max-width: 90vw;
  height: 400px;
  background-color: rgba(15, 15, 15, 0.5);
  backdrop-filter: blur(20px);
  -webkit-backdrop-filter: blur(20px);
  border-radius: 12px;
  border: 1px solid rgba(255, 255, 255, 0.1);
  box-shadow: 0 8px 32px rgba(0, 0, 0, 0.3);
  display: flex;
  flex-direction: column;
  margin-top: 2rem;
  font-family: 'Fira Code', monospace;
}

.title-bar {
  background-color: rgba(46, 46, 46, 0.5) d;
  padding: 0 12px; /* Add some horizontal padding */
  border-top-left-radius: 10px;
  border-top-right-radius: 10px;
  display: flex;
  align-items: center; /* Vertically center content */
  justify-content: space-between; /* This pushes tabs left and buttons right */
  position: relative;
  height: 56px; /* Increased height for a more spacious feel */
}

.buttons {
  display: flex;
  gap: 8px;
  margin-right: 8px;
  /* No margin needed anymore */
}
.btn {
  width: 12px;
  height: 12px;
  border-radius: 50%;
}
.close {
  background-color: #ff5f56;
}
.minimize {
  background-color: #ffbd2e;
}
.maximize {
  background-color: #27c93f;
}

.tabs-container {
  display: flex;
  flex-grow: 1;
  align-items: flex-end; /* Align tabs to the bottom of the bar */
  height: 100%; /* Make container full height of title bar */
}

.tab-item {
  background-color: rgba(46, 46, 46, 0.5);
  color: #ccc;
  padding: 10px 18px; /* Added more padding */
  cursor: pointer;
  position: relative;
  display: flex;
  align-items: center;
  gap: 64px;
  font-size: 0.8rem;
  border: 1px solid transparent;
  border-bottom: none;
  /* The large border-radius you requested */
  border-top-left-radius: 0.6rem;
  border-top-right-radius: 0.6rem;
}

.tab-item.active {
  background-color: rgba(15, 15, 15, 0.5);
  color: white;
  border-color: rgba(255, 255, 255, 0.1);
}

.close-tab-btn {
  width: 20px;
  height: 20px;
  border-radius: 50%;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 20px;
  line-height: 1;
}
.close-tab-btn:hover {
  background-color: rgba(255, 255, 255, 0.1);
}

.new-tab-btn {
  width: 38px; /* Increased size to better match tab height */
  height: 38px; /* Increased size to better match tab height */
  border-radius: 6px;
  background-color: rgba(46, 46, 46, 0.5);
  color: white;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 1.5rem;

  cursor: pointer;
  margin-left: 8px;
}
.new-tab-btn:hover {
  background-color: rgba(255, 255, 255, 0.1);
}

/* ... The rest of the styles are the same ... */

/* --- Body and Input Styling (mostly the same) --- */
.terminal-body {
  flex-grow: 1;
  padding: 10px;
  overflow-y: auto;
  color: #f0f0f0;
  font-size: 0.9rem;
  line-height: 1.5;
  border-top: 1px solid rgba(255, 255, 255, 0.1);
}
.line {
  white-space: pre-wrap;
  word-wrap: break-word;
}
.prompt {
  color: #76e3ea;
  font-weight: bold;
  margin-right: 8px;
}
.input-line {
  display: flex;
}
input {
  flex-grow: 1;
  background: none;
  border: none;
  color: #f0f0f0;
  font-family: 'Fira Code', monospace;
  font-size: 0.9rem;
}
input:focus {
  outline: none;
}
@media (max-width: 768px) {
  .terminal-window {
    /* Make terminal take up more of the screen on mobile */
    width: 95vw;
    height: 350px;
    margin-top: 1rem;
  }

  .title-bar {
    height: 48px; /* Slightly smaller title bar */
    padding: 0 8px;
  }

  .tab-item {
    padding: 8px 10px; /* Smaller padding on tabs */
    font-size: 0.75rem;
    gap: 60px; /* Less gap between text and close button */
  }

  .close-tab-btn {
    width: 14px;
    height: 14px;
    font-size: 20px;
  }

  .new-tab-btn {
    width: 32px;
    height: 32px;
    font-size: 1.2rem;
  }

  .prompt,
  input,
  .line {
    font-size: 0.85rem; /* Slightly smaller text in the terminal body */
  }
}
</style>
