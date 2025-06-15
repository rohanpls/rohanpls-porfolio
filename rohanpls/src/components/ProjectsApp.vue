<template>
  <div class="projects-app-container">
    <h1 class="title">Limited on Github</h1>
    <div class="cards-container">
      <Card v-for="card in cards" :key="card.title" :data-image="card.image" :url="card.url">
        <template #header
          ><h1>{{ card.title }}</h1></template
        >
        <template #content
          ><p>{{ card.content }}</p></template
        >
      </Card>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import Card from './ACard.vue'

import imageOne from '@/assets/images/image-1.png'
import imageTwo from '@/assets/images/image-2.png'
import imageThree from '@/assets/images/image-3.png'
import imageFour from '@/assets/images/image-4.png'

const cards = ref([
  {
    title: 'InstaQR Generator',
    content: 'Instant QR Code generator using JS',
    image: imageOne,
    url: 'https://github.com/rohanpls/instaqr',
  },
  {
    title: 'Shell Greeter',
    content: 'Ricing for your terminal, make it pretty!',
    image: imageTwo,
    url: 'https://github.com/rohanpls/shellgreeter',
  },
  {
    title: 'rohanpls Portfolio',
    content: 'This portfolio is made using Vue 3, which you are viewing right now!',
    image: imageThree,
    url: 'https://github.com/rohanpls/rohanpls-portfolio',
  },
])
</script>

<style lang="scss" scoped>
.projects-app-container {
  font-family: 'Montserrat', sans-serif;
  background-color: #7e83c7;
  -webkit-font-smoothing: antialiased;
  width: 100%;
  height: 100%;
  overflow-y: auto;
  border-radius: 5px;
}

.title {
  padding-top: 20px;
  font-family: 'Montserrat', sans-serif;
  font-size: 24px;
  font-weight: 700;
  color: #5d4037;
  text-align: center;
}

.cards-container {
  padding: 20px 40px;
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
}
</style>
