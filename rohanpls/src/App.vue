<script setup>
import { ref, provide, shallowRef, onMounted, onUnmounted } from 'vue'
import MainDisplay from './components/MainDisplay.vue'
import AppWindow from './components/AppWindow.vue'
import AppDock from './components/AppDock.vue'
import MobileSplash from './components/MobileSplash.vue'
import { dockIcons } from './components/dockIcons.js'

import ProjectsApp from './components/ProjectsApp.vue'
import SkillsApp from './components/SkillsApp.vue'
import TicTacToeApp from './components/TicTacToeApp.vue'
import ContactApp from './components/ContactApp.vue'

const isMobile = ref(false)

const checkScreenSize = () => {
  isMobile.value = window.innerWidth <= 768
}

onMounted(() => {
  checkScreenSize()
  window.addEventListener('resize', checkScreenSize)
})
onUnmounted(() => {
  window.removeEventListener('resize', checkScreenSize)
})

const appComponents = {
  projects: shallowRef(ProjectsApp),
  skills: shallowRef(SkillsApp),
  tictactoe: shallowRef(TicTacToeApp),
  contact: shallowRef(ContactApp),
}

const dockApps = ref([
  { name: 'close-all', title: 'Home | Close All Apps', icon: dockIcons.closeAll },
  { name: 'terminal', title: 'Terminal', icon: dockIcons.terminal },
  { name: 'projects', title: 'Projects', icon: dockIcons.projects },
  { name: 'skills', title: 'Skills', icon: dockIcons.skills },
  { name: 'tictactoe', title: 'Tic-Tac-Toe', icon: dockIcons.tictactoe },
  { name: 'contact', title: 'Contact', icon: dockIcons.contact },
])

const openWindows = ref([])
let nextZIndex = ref(10)

const openApp = (appName) => {
  if (appName === 'close-all') {
    closeAllWindows()
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
    isClosing: false,
  }
  openWindows.value.push(newWindow)
}

const closeApp = (id) => {
  const index = openWindows.value.findIndex((w) => w.id === id)
  if (index !== -1) {
    openWindows.value[index].isClosing = true
    setTimeout(() => {
      openWindows.value.splice(index, 1)
    }, 300)
  }
}

const bringWindowToFront = (id) => {
  const window = openWindows.value.find((w) => w.id === id)
  if (window) {
    window.zIndex = nextZIndex.value++
  }
}

const closeAllWindows = () => {
  openWindows.value = []
}

provide('openApp', openApp)
</script>

<template>
  <MobileSplash v-if="isMobile" />

  <template v-else>
    <MainDisplay />

    <AppDock :apps="dockApps" @launch-app="openApp" />

    <transition-group name="window-zoom">
      <AppWindow
        v-for="window in openWindows"
        :key="window.id"
        v-show="!window.isClosing"
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
