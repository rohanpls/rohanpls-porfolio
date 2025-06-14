<script setup>
// Its only job is to import your main page component.
import MainDisplay from './components/MainDisplay.vue'
</script>

<template>
  <MainDisplay />
</template>

<style>
/* This is a good place for truly global styles that affect the whole site, like body margins. */
body {
  margin: 0;
  padding: 0;
}
</style>