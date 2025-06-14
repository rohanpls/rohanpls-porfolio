<template>
  <div class="terminal-window" @click="focusInput">
    <div class="title-bar">
      <div class="buttons">
        <div class="btn close"></div>
        <div class="btn minimize"></div>
        <div class="btn maximize"></div>
      </div>
      <div class="title">rohanpls — -zsh</div>
    </div>
    <div class="terminal-body" ref="terminalBody">
      <div v-for="(line, index) in history" :key="index" class="line">
        <span v-if="line.type === 'command'" class="prompt">{{ prompt }}</span>
        <span>{{ line.text }}</span>
      </div>
      <div class="input-line">
        <span class="prompt">{{ prompt }}</span>
        <input
          ref="inputField"
          type="text"
          v-model="input"
          @keydown.enter="handleCommand"
          autofocus
        />
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, watch, nextTick } from 'vue';

const input = ref('');
const history = ref([
  { type: 'output', text: "Welcome! Type 'help' to see available commands." }
]);
const prompt = 'rohanpls@portfolio:~$';
const inputField = ref(null);
const terminalBody = ref(null);

// Command definitions
const commands = {
  help: () => [
    "'whoami'   - Information about me",
    "'projects' - View my latest projects",
    "'contact'  - How to get in touch",
    "'date'     - Displays the current date",
    "'clear'    - Clears the terminal history"
  ],
  whoami: () => [
    "Rohan, a passionate developer creating modern web experiences.",
    "I specialize in frontend technologies and love building cool, interactive UIs."
  ],
  projects: () => [
    "Project 1: A super-secret app built with Vue and love.",
    "Project 2: An e-commerce site that sells digital kittens.",
    "Project 3: This very portfolio terminal!"
  ],
  contact: () => [
    "You can reach me via:",
    "Email: your.email@example.com",
    "LinkedIn: linkedin.com/in/yourprofile"
  ],
  date: () => new Date().toLocaleString(),
  clear: () => {
    history.value = [];
    return []; // Return empty array so nothing is printed
  }
};

const handleCommand = () => {
  const command = input.value.trim().toLowerCase();
  // Add the command to history
  history.value.push({ type: 'command', text: command });

  if (command in commands) {
    const output = commands[command]();
    if (Array.isArray(output)) {
      history.value.push(...output.map(text => ({ type: 'output', text })));
    } else {
      history.value.push({ type: 'output', text: output });
    }
  } else if (command !== '') {
    history.value.push({ type: 'output', text: `zsh: command not found: ${command}` });
  }

  // Clear input
  input.value = '';
};

const focusInput = () => {
  inputField.value?.focus();
};

// Watch for changes in history and scroll to the bottom
watch(history, async () => {
  await nextTick();
  if (terminalBody.value) {
    terminalBody.value.scrollTop = terminalBody.value.scrollHeight;
  }
}, { deep: true });
</script>

<style scoped>
/* ... inside the <style scoped> block ... */

.terminal-window {
  width: 700px;
  max-width: 90vw;
  height: 400px;
  
  /* --- ADJUSTED FOR iOS BLUR --- */
  /* Make the background more transparent to see the blur better */
  background-color: rgba(40, 45, 60, 0.65); 
  
  /* The magic property! Increase the blur value for a stronger effect. */
  backdrop-filter: blur(20px);
  -webkit-backdrop-filter: blur(20px); /* For better browser compatibility */
  
  border-radius: 12px;
  border: 1px solid rgba(255, 255, 255, 0.1);
  box-shadow: 0 8px 32px rgba(0, 0, 0, 0.3);
  display: flex;
  flex-direction: column;
  margin-top: 2rem;
  font-family: 'Fira Code', monospace;
}

/* ... the rest of your styles ... */

.title-bar {
  background-color: #3a3d4a;
  padding: 8px;
  border-top-left-radius: 10px;
  border-top-right-radius: 10px;
  display: flex;
  align-items: center;
  position: relative;
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
.close { background-color: #ff5f56; }
.minimize { background-color: #ffbd2e; }
.maximize { background-color: #27c93f; }

.title {
  color: #ccc;
  font-size: 0.8rem;
  position: absolute;
  left: 50%;
  transform: translateX(-50%);
}

.terminal-body {
  flex-grow: 1;
  padding: 10px;
  overflow-y: auto;
  color: #f0f0f0;
  font-size: 0.9rem;
  line-height: 1.5;
}

.line {
  white-space: pre-wrap; /* Allows text to wrap */
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
</style>