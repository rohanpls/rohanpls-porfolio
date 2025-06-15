<script setup>
import { ref, provide, shallowRef } from 'vue'
import MainDisplay from './components/MainDisplay.vue'
import AppWindow from './components/AppWindow.vue'
import AppDock from './components/AppDock.vue'
import { dockIcons } from './components/dockIcons.js'
import ProjectsApp from './components/ProjectsApp.vue'
import SkillsApp from './components/SkillsApp.vue'
import TicTacToeApp from './components/TicTacToeApp.vue'
import ContactApp from './components/ContactApp.vue'

const openWindows = ref([])
let nextZIndex = ref(10)

const terminal = ref({
  id: 'main-terminal',
  isVisible: true,
  zIndex: 11,
})

const appComponents = {
  projects: shallowRef(ProjectsApp),
  skills: shallowRef(SkillsApp),
  tictactoe: shallowRef(TicTacToeApp),
  contact: shallowRef(ContactApp),
}

const dockApps = ref([
  { name: 'terminal', title: 'Terminal', icon: dockIcons.terminal },
  { name: 'projects', title: 'Projects', icon: dockIcons.projects },
  { name: 'skills', title: 'Skills', icon: dockIcons.skills },
  { name: 'tictactoe', title: 'Tic-Tac-Toe', icon: dockIcons.tictactoe },
  { name: 'contact', title: 'Contact', icon: dockIcons.contact },
])
const bringWindowToFront = (id) => {
  const windowToFocus =
    openWindows.value.find((w) => w.id === id) || (id === 'main-terminal' ? terminal.value : null)
  if (windowToFocus) {
    windowToFocus.zIndex = nextZIndex.value++
  }
}

const openApp = (appName) => {
  if (appName === 'terminal') {
    if (terminal.value.isVisible) {
      bringWindowToFront('main-terminal')
    } else {
      terminal.value.isVisible = true
      setTimeout(() => bringWindowToFront('main-terminal'), 0)
    }
    return
  }

  const existingWindow = openWindows.value.find((w) => w.appName === appName)
  if (existingWindow) {
    bringWindowToFront(existingWindow.id)
    return
  }

  const appTitles = {
    projects: 'Projects',
    skills: 'Skills & Proficiencies',
    tictactoe: 'Tic Tac Toe',
    contact: 'Contact Card',
  }

  const newWindow = {
    id: Date.now(),
    appName: appName,
    component: appComponents[appName],
    title: appTitles[appName] || 'App',
    zIndex: nextZIndex.value++,
    position: { x: window.innerWidth / 2 - 400, y: window.innerHeight / 2 - 250 },
  }
  openWindows.value.push(newWindow)
}

const closeApp = (id) => {
  const index = openWindows.value.findIndex((w) => w.id === id)
  if (index !== -1) openWindows.value.splice(index, 1)
}

provide('openApp', openApp)
provide('focusWindow', bringWindowToFront)
</script>

<template>
  <MainDisplay
    :is-terminal-visible="terminal.isVisible"
    :terminal-z-index="terminal.zIndex"
    @focus-terminal="bringWindowToFront('main-terminal')"
  />

  <AppDock :apps="dockApps" @launch-app="openApp" />

  <transition-group name="window-zoom">
    <AppWindow
      v-for="window in openWindows"
      :key="window.id"
      :title="window.title"
      :initialPosition="window.position"
      :zIndex="window.zIndex"
      @close="closeApp(window.id)"
      @focus="bringWindowToFront(window.id)"
    >
      <component :is="window.component" />
    </AppWindow>
  </transition-group>
</template>

<style>
body {
  margin: 0;
  padding: 0;
  background-color: #0d1117;
}
.window-zoom-enter-active,
.window-zoom-leave-active {
  transition: all 0.3s ease;
}
.window-zoom-enter-from,
.window-zoom-leave-to {
  opacity: 0;
  transform: scale(0.3);
}
</style>
