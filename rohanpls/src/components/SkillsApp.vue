<template>
  <div class="skills-container">
    <div v-for="category in skillCategories" :key="category.title" class="category-section">
      <h2>{{ category.title }}</h2>
      <ul class="honeycomb">
        <li v-for="skill in category.skills" :key="skill.name" class="honeycomb-cell">
          <div class="cell-bg">
             <i :class="skill.icon"></i>
          </div>
          <div class="cell-title">{{ skill.name }}</div>
        </li>
      </ul>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue';

const skillCategories = ref([
  {
    title: 'Languages & Runtimes',
    skills: [
      { name: 'Java', icon: 'devicon-java-plain' },
      { name: 'Python', icon: 'devicon-python-plain' },
      { name: 'TypeScript', icon: 'devicon-typescript-plain' },
      { name: 'Node.js', icon: 'devicon-nodejs-plain' }
    ]
  },
  {
    title: 'Frameworks & Libraries',
    skills: [
      { name: 'Spring Boot', icon: 'devicon-spring-plain' },
      { name: 'React', icon: 'devicon-react-original' },
      // --- CHANGE: Replaced Angular with Vue.js ---
      { name: 'Vue.js', icon: 'devicon-vuejs-plain' },
      { name: 'Express', icon: 'devicon-express-original' }
      // --- CHANGE: Hibernate has been removed ---
    ]
  },
  {
    title: 'Databases',
    skills: [
      { name: 'PostgreSQL', icon: 'devicon-postgresql-plain' },
      { name: 'DynamoDB', icon: 'devicon-amazonwebservices-plain' },
      // --- CHANGE: SQL icon is now MySQL ---
      { name: 'SQL', icon: 'devicon-mysql-plain' },
      { name: 'NoSQL', icon: 'devicon-mongodb-plain' }
    ]
  },
  {
    title: 'DevOps & Cloud',
    skills: [
      { name: 'AWS', icon: 'devicon-amazonwebservices-plain' },
      { name: 'Docker', icon: 'devicon-docker-plain' },
      { name: 'Jenkins', icon: 'devicon-jenkins-line' },
      { name: 'Git', icon: 'devicon-git-plain' },
      { name: 'CI/CD', icon: 'devicon-gitlab-plain' }
    ]
  }
]);
</script>

<style lang="scss" scoped>
.skills-container {
  background-color: #1e1e1e;
  border-radius: 5px;
  padding: 20px;
  overflow-y: auto;
  color: #f0f0f0;
}

.category-section {
  margin-bottom: 1.5rem;
  h2 {
    text-align: center;
    font-family: 'Montserrat', sans-serif;
    color: #76e3ea;
    margin-bottom: 1rem;
    font-size: 1.2rem;
  }
}

.honeycomb {
  display: flex;
  flex-wrap: wrap;
  list-style: none;
  justify-content: center;
  padding: 0;
  margin: 0 auto;
  max-width: 650px;
}

.honeycomb-cell {
  flex: 0 0 110px;
  max-width: 110px;
  height: 125px;
  margin: 18px 5px 0;
  position: relative;
  text-align: center;
  transition: transform 0.3s ease;

  .cell-bg {
    position: absolute;
    top: 0;
    left: 12.5px;
    width: 85px;
    height: 100%;
    display: flex;
    align-items: center;
    justify-content: center;
    background-color: #2c2c2c;
    clip-path: polygon(50% 0%, 100% 25%, 100% 75%, 50% 100%, 0% 75%, 0% 25%);
    transition: background-color 0.3s ease;
  }

  i {
    font-size: 3rem;
    color: #ccc;
    transition: color 0.3s ease;
  }
  
  .cell-title {
    position: absolute;
    bottom: 20px;
    width: 100%;
    padding: 5px 0;
    font-family: 'Montserrat', sans-serif;
    font-size: 0.75rem;
    color: #ccc;
    opacity: 0;
    transition: opacity 0.3s ease;
  }

  &:hover {
    transform: scale(1.1);
    .cell-bg {
      background-color: #3B82F6;
    }
    .cell-title {
      opacity: 1;
    }
    
    /* --- HOVER FIX: Make selector more specific --- */
    .cell-bg i {
      color: white;
    }
  }
}
</style>