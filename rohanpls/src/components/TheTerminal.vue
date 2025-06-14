<template>
  <div class="terminal-window" @click="focusInput">
    <div class="title-bar">
      <div class="buttons">
        <div class="btn close"></div>
        <div class="btn minimize"></div>
        <div class="btn maximize"></div>
      </div>
      
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
        <div class="new-tab-btn" @click="addTab">+</div>
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
import { ref, computed, watch, nextTick, onMounted } from 'vue';

// --- State Refactor for Tabs ---
const tabs = ref([]);
const prompt = 'rohanpls@portfolio:~$';
const inputField = ref(null);
const terminalBody = ref(null);

// A computed property to easily get the currently active tab
const currentTab = computed(() => tabs.value.find(tab => tab.isActive));

// --- Core Tab Management Functions ---
const createNewTab = () => ({
  id: Date.now(),
  history: [{ type: 'output', text: "Welcome! Type 'help' to see commands." }],
  input: '',
  isActive: false
});

const addTab = () => {
  tabs.value.forEach(t => t.isActive = false);
  const newTab = createNewTab();
  newTab.isActive = true;
  tabs.value.push(newTab);
};

const selectTab = (id) => {
  tabs.value.forEach(t => t.isActive = t.id === id);
};

const closeTab = (id) => {
  const index = tabs.value.findIndex(t => t.id === id);
  if (index === -1) return;

  const wasActive = tabs.value[index].isActive;
  tabs.value.splice(index, 1);

  if (wasActive && tabs.value.length > 0) {
    // Activate the previous tab, or the first one if the closed tab was the first
    const newActiveIndex = Math.max(0, index - 1);
    tabs.value[newActiveIndex].isActive = true;
  } else if (tabs.value.length === 0) {
    // If all tabs are closed, open a new default one
    addTab();
  }
};

// Initialize with one tab on mount
onMounted(() => {
  addTab();
});

// --- Command Logic (now operates on the current tab) ---
const commands = {
  help: () => ["'whoami'", "'projects'", "'contact'", "'date'", "'clear'"],
  whoami: () => ["Rohan, a passionate developer."],
  projects: () => ["Project 1: This portfolio!", "Project 2: Secret AI app."],
  contact: () => ["Email: your.email@example.com", "LinkedIn: linkedin.com/in/yourprofile"],
  date: () => new Date().toLocaleString(),
  clear: () => {
    if(currentTab.value) currentTab.value.history = [];
    return [];
  }
};

const handleCommand = () => {
  if (!currentTab.value) return;
  const tab = currentTab.value;
  const command = tab.input.trim().toLowerCase();

  tab.history.push({ type: 'command', text: command });

  if (command in commands) {
    const output = commands[command]();
    if (Array.isArray(output)) tab.history.push(...output.map(text => ({ type: 'output', text })));
    else tab.history.push({ type: 'output', text: output });
  } else if (command !== '') {
    tab.history.push({ type: 'output', text: `zsh: command not found: ${command}` });
  }
  tab.input = '';
};

// --- Helper Functions ---
const focusInput = () => inputField.value?.focus();

// Watch for changes in history of the current tab to scroll down
watch(() => currentTab.value?.history, async () => {
  await nextTick();
  if (terminalBody.value) terminalBody.value.scrollTop = terminalBody.value.scrollHeight;
}, { deep: true });
</script>

<style scoped>
/* Main window styling remains similar */
.terminal-window {
  width: 700px;
  max-width: 90vw;
  height: 400px;
  background-color: rgba(40, 45, 60, 0.65);
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

/* --- Title Bar and Tabs Styling --- */
.title-bar {
  background-color: #2e2e3a; /* Slightly different color for the whole bar */
  padding: 0 8px; /* Remove vertical padding, manage height with content */
  border-top-left-radius: 10px;
  border-top-right-radius: 10px;
  display: flex;
  align-items: flex-end; /* Align items to the bottom of the bar */
  position: relative;
  height: 44px; /* Set a fixed height for the bar */
}

.buttons {
  display: flex;
  gap: 8px;
  margin-right: 16px;
  align-self: center; /* Center the buttons vertically */
}
.btn { width: 12px; height: 12px; border-radius: 50%; }
.close { background-color: #ff5f56; }
.minimize { background-color: #ffbd2e; }
.maximize { background-color: #27c93f; }

.tabs-container {
  display: flex;
  flex-grow: 1;
  align-items: flex-end;
}

.tab-item {
  background-color: #4a4a5a;
  color: #ccc;
  padding: 8px 12px;
  border-top-left-radius: 6px;
  border-top-right-radius: 6px;
  cursor: pointer;
  position: relative;
  bottom: -1px; /* To make it overlap the border */
  display: flex;
  align-items: center;
  gap: 8px;
  font-size: 0.8rem;
  border: 1px solid transparent;
  border-bottom: none;
}

.tab-item.active {
  background-color: rgba(40, 45, 60, 0.65); /* Match terminal body background */
  color: white;
  border-color: rgba(255, 255, 255, 0.1);
}

.close-tab-btn {
  width: 16px;
  height: 16px;
  border-radius: 50%;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 12px;
  line-height: 1;
}
.close-tab-btn:hover {
  background-color: rgba(255, 255, 255, 0.1);
}

.new-tab-btn {
  width: 28px;
  height: 28px;
  border-radius: 6px;
  background-color: rgba(255, 255, 255, 0.05);
  color: white;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 1.2rem;
  cursor: pointer;
  margin-left: 8px;
  margin-bottom: 2px;
  align-self: center;
}
.new-tab-btn:hover {
  background-color: rgba(255, 255, 255, 0.1);
}

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
.line { white-space: pre-wrap; word-wrap: break-word; }
.prompt { color: #76e3ea; font-weight: bold; margin-right: 8px; }
.input-line { display: flex; }
input { flex-grow: 1; background: none; border: none; color: #f0f0f0; font-family: 'Fira Code', monospace; font-size: 0.9rem; }
input:focus { outline: none; }
</style>