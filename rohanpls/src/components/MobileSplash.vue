<template>
  <div class="splash-container">
    <div class="content">
      <img src="../assets/images/ricebowl.png" alt="Logo" class="logo-image" />
      <h1>Rohan S.</h1>
      <p class="subtitle">Software Development Engineer & DevOps Engineer</p>
      <p class="message">Thank you for visiting my interactive portfolio.</p>
      <div class="desktop-prompt">
        <svg
          fill="none"
          stroke="currentColor"
          stroke-width="1.5"
          viewBox="0 0 24 24"
          xmlns="http://www.w3.org/2000/svg"
        >
          <path
            stroke-linecap="round"
            stroke-linejoin="round"
            d="M9 17.25v1.007a3 3 0 01-.879 2.122L7.5 21h9l-1.621-.879a3 3 0 01-.879-2.122v-1.007M15 9.75a3 3 0 11-6 0 3 3 0 016 0z"
          ></path>
          <path
            stroke-linecap="round"
            stroke-linejoin="round"
            d="M12 12.75c-1.657 0-3-1.343-3-3s1.343-3 3-3 3 1.343 3 3-1.343 3-3 3z"
          ></path>
          <path
            stroke-linecap="round"
            stroke-linejoin="round"
            d="M2.25 15.75l.41-3.359A12.09 12.09 0 0112 8.25c4.544 0 8.525 2.134 11.34 5.353l.41 3.359m-1.84 3.359a12.09 12.09 0 00-11.34-5.353c-4.544 0-8.525-2.134-11.34-5.353m1.84-3.359L2.25 6.75A12.09 12.09 0 0112 3c4.544 0 8.525 2.134 11.34 5.353l.41 3.359"
          ></path>
        </svg>
        <span>For the full interactive experience, please visit on a desktop or laptop.</span>
      </div>
    </div>
  </div>
</template>

<style scoped>
.splash-container {
  width: 100vw;
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: #0d1117;
  color: white;
  font-family: 'Montserrat', sans-serif;
  text-align: center;
  padding: 20px;
}

.content {
  max-width: 400px;
}

.logo-image {
  width: 100px;
  height: auto;
  margin-bottom: 1rem;
}

h1 {
  font-size: 2rem;
  font-weight: 700;
  margin: 0;
}

.subtitle {
  font-size: 1rem;
  font-weight: 400;
  color: #ccc;
  margin: 0.5rem 0 1.5rem;
}

.message {
  font-family: 'Raleway', sans-serif;
  color: #aeb3b8;
  line-height: 1.6;
}

.desktop-prompt {
  margin-top: 2rem;
  padding: 1rem;
  background-color: rgba(40, 45, 60, 0.5);
  border-radius: 12px;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 1rem;
  font-size: 0.9rem;
  color: #ccc;
}

.desktop-prompt svg {
  width: 40px;
  height: 40px;
  color: #76e3ea;
}
</style>
