<script setup>
import { ref, provide, shallowRef } from 'vue';
import MainDisplay from './components/MainDisplay.vue';
import AppWindow from './components/AppWindow.vue';
import ProjectsApp from './components/ProjectsApp.vue';
import SkillsApp from './components/SkillsApp.vue';

const appComponents = {
  projects: shallowRef(ProjectsApp),
  skills: shallowRef(SkillsApp)
};

const openWindows = ref([]);
let nextZIndex = ref(10);

const openApp = (appName) => {
  const existingWindow = openWindows.value.find(w => w.appName === appName);
  if (existingWindow) {
    bringWindowToFront(existingWindow.id);
    return;
  }
  const appTitles = {
    projects: 'Projects',
    skills: 'Skills & Proficiencies'
  };
  const newWindow = {
    id: Date.now(),
    appName: appName,
    component: appComponents[appName],
    title: appTitles[appName] || 'App',
    zIndex: nextZIndex.value++,
    position: { x: window.innerWidth / 2 - 400, y: window.innerHeight / 2 - 250 },
    isClosing: false
  };
  openWindows.value.push(newWindow);
};

const closeApp = (id) => {
  const index = openWindows.value.findIndex(w => w.id === id);
  if (index !== -1) {
    openWindows.value[index].isClosing = true;
    setTimeout(() => {
      openWindows.value.splice(index, 1);
    }, 300);
  }
};

const bringWindowToFront = (id) => {
  const window = openWindows.value.find(w => w.id === id);
  if (window) {
    window.zIndex = nextZIndex.value++;
  }
};

provide('openApp', openApp);
</script>

<template>
  <MainDisplay />

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

<style>
body { margin: 0; padding: 0; background-color: #0d1117; }

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