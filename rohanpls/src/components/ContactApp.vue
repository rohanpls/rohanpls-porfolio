<template>
  <div class="contact-app-container">
    <div class="flip-card">
      <div class="card-inner">
        <div class="card-front">
          <p>Rohan S.</p>
        </div>

        <div class="card-back">
          <div class="contact-details">
            <p class="email-label">EMAIL</p>
            <a class="email-address" href="mailto:dev.rohanpls@gmail.com">
              dev.rohanpls@gmail.com
            </a>
          </div>
          <div class="social-links">
            <a href="https://github.com/rohanpls" target="_blank" class="social-btn github">
              <svg role="img" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
                <title>GitHub</title>
                <path
                  d="M12 .297c-6.63 0-12 5.373-12 12 0 5.303 3.438 9.8 8.205 11.385.6.113.82-.258.82-.577 0-.285-.01-1.04-.015-2.04-3.338.724-4.042-1.61-4.042-1.61C4.422 18.07 3.633 17.7 3.633 17.7c-1.087-.744.084-.729.084-.729 1.205.084 1.838 1.236 1.838 1.236 1.07 1.835 2.809 1.305 3.495.998.108-.776.417-1.305.76-1.605-2.665-.3-5.466-1.332-5.466-5.93 0-1.31.465-2.38 1.235-3.22-.135-.303-.54-1.523.105-3.176 0 0 1.005-.322 3.3 1.23.96-.267 1.98-.399 3-.405 1.02.006 2.04.138 3 .405 2.28-1.552 3.285-1.23 3.285-1.23.645 1.653.24 2.873.12 3.176.765.84 1.23 1.91 1.23 3.22 0 4.61-2.805 5.625-5.475 5.92.42.36.81 1.096.81 2.22 0 1.606-.015 2.896-.015 3.286 0 .315.21.69.825.57C20.565 22.092 24 17.592 24 12.297c0-6.627-5.373-12-12-12"
                />
              </svg>
              <span>GitHub</span>
            </a>
            <a
              href="https://www.linkedin.com/in/rohanpls"
              target="_blank"
              class="social-btn linkedin"
            >
              <svg role="img" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
                <title>LinkedIn</title>
                <path
                  d="M20.447 20.452h-3.554v-5.569c0-1.328-.027-3.037-1.852-3.037-1.853 0-2.136 1.445-2.136 2.939v5.667H9.351V9h3.414v1.561h.046c.477-.9 1.637-1.85 3.37-1.85 3.601 0 4.267 2.37 4.267 5.455v6.286zM5.337 7.433a2.062 2.062 0 0 1-2.063-2.065 2.064 2.064 0 1 1 2.063 2.065zm1.782 13.019H3.555V9h3.564v11.452zM22.225 0H1.771C.792 0 0 .774 0 1.729v20.542C0 23.227.792 24 1.771 24h20.451C23.2 24 24 23.227 24 22.271V1.729C24 .774 23.2 0 22.225 0z"
                />
              </svg>
              <span>LinkedIn</span>
            </a>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.contact-app-container {
  height: 90%;
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: rgba(15, 15, 15, 0.1);
  backdrop-filter: blur(20px);
  padding: 20px;
  border-radius: 5px;
  font-family: 'Raleway', sans-serif;
  overflow: hidden;
}

.flip-card {
  background-color: transparent;
  width: 420px;
  height: 240px;
  perspective: 1000px;
}

.card-inner {
  position: relative;
  width: 100%;
  height: 100%;
  text-align: center;
  transition: transform 0.6s;
  transform-style: preserve-3d;
}

.flip-card:hover .card-inner {
  transform: rotateY(180deg);
}

.card-front,
.card-back {
  position: absolute;
  width: 100%;
  height: 100%;
  -webkit-backface-visibility: hidden;
  backface-visibility: hidden;
  border-radius: 12px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

.card-front {
  background: linear-gradient(145deg, #f9f9f9, #ffffff);
  border: 1px solid rgba(255, 255, 255, 0.5);
  color: #2c3e50;
}

.card-front p {
  font-size: 3rem;
  font-weight: 700;
}

.card-back {
  background: linear-gradient(145deg, #2c3e50, #34495e);
  color: white;
  transform: rotateY(180deg);
  padding: 20px;
}

.email-label {
  font-size: 0.8rem;
  text-transform: uppercase;
  letter-spacing: 1px;
  opacity: 0.7;
}

.email-address {
  font-size: 1.2rem;
  font-weight: bold;
  color: white;
  text-decoration: none;
  margin-top: 5px;
}

.social-links {
  margin-top: 25px;
  display: flex;
  gap: 15px;
  border-top: 1px solid rgba(255, 255, 255, 0.2);
  padding-top: 25px;
}

.social-btn {
  display: flex;
  align-items: center;
  gap: 8px;
  padding: 8px 15px;
  background-color: rgba(255, 255, 255, 0.1);
  border-radius: 8px;
  text-decoration: none;
  color: white;
  transition: background-color 0.2s;
}

.social-btn:hover {
  background-color: rgba(255, 255, 255, 0.2);
}

.social-btn svg {
  width: 20px;
  height: 20px;
  fill: white;
}
</style>
