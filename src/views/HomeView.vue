<template>
  <div class="home">
    <div class="title-container">
      <Particles
        id="tsparticles"
        :particlesInit="particlesInit"
        :particlesLoaded="particlesLoaded"
        :options="particlesOptions"
      />
      <h1 class="name">XuMJ</h1>
    </div>
    <div class="content">
      <p class="subtitle">Personal Website | 个人网站</p>
      <p class="tagline">创新 · 分享 · 连接</p>
      <h2 class="welcome">Welcome to My Personal Website | 欢迎来到我的个人网站</h2>
      <p class="description">Sharing Stories, Ideas & Inspirations | 分享故事、想法与灵感</p>
    </div>
    <div class="scroll-down-arrow"></div>
  </div>
</template>

<script setup>
import { onMounted } from 'vue'
import gsap from 'gsap'
import { loadFull } from 'tsparticles';

const particlesInit = async (engine) => {
  await loadFull(engine)
}

const particlesLoaded = async (container) => {
  console.log('Particles container loaded', container)
}

const particlesOptions = {
  fullScreen: {
    enable: false,
    zIndex: 0
  },
  fpsLimit: 60,
  interactivity: {
    events: {
      onClick: {
        enable: true,
        mode: 'push'
      },
      onHover: {
        enable: true,
        mode: 'repulse'
      },
      resize: true
    },
    modes: {
      push: {
        quantity: 4
      },
      repulse: {
        distance: 200,
        duration: 0.4
      }
    }
  },
  particles: {
    color: {
      value: '#ffffff'
    },
    links: {
      color: '#ffffff',
      distance: 150,
      enable: true,
      opacity: 0.5,
      width: 1
    },
    collisions: {
      enable: true
    },
    move: {
      direction: 'none',
      enable: true,
      outModes: {
        default: 'bounce'
      },
      random: false,
      speed: 2,
      straight: false
    },
    number: {
      density: {
        enable: true,
        area: 800
      },
      value: 80
    },
    opacity: {
      value: 0.5
    },
    shape: {
      type: 'circle'
    },
    size: {
      value: { min: 1, max: 5 }
    }
  },
  detectRetina: true
};

onMounted(() => {
  const tl = gsap.timeline();
      tl.from(".name", { duration: 1.5, y: -100, opacity: 0, ease: 'power4.out', scale: 1.2 })
    .from('.subtitle', { duration: 1, y: -30, opacity: 0, ease: 'power3.out' }, '-=1')
    .from('.tagline', { duration: 1, y: -20, opacity: 0, ease: 'power3.out' }, '-=0.8')
    .from('.welcome', { duration: 1.5, scale: 0.5, opacity: 0, ease: 'elastic.out(1, 0.5)' }, '+=0.2')
    .from('.description', { duration: 1, y: 20, opacity: 0, ease: 'power3.out' }, '-=1')
    .from('.scroll-down-arrow', { duration: 1, y: -20, opacity: 0, ease: 'bounce.out' }, '+=0.5');
});
</script>

<style scoped>
.scroll-down-arrow {
  position: absolute;
  bottom: 30px;
  left: 50%;
  transform: translateX(-50%);
  width: 24px;
  height: 24px;
  border-left: 2px solid #fff;
  border-bottom: 2px solid #fff;
  transform: rotate(-45deg);
  animation: bounce 2s infinite;
  opacity: 0.7;
}

@keyframes bounce {
  0%, 20%, 50%, 80%, 100% {
    transform: translateY(0) rotate(-45deg);
  }
  40% {
    transform: translateY(-20px) rotate(-45deg);
  }
  60% {
    transform: translateY(-10px) rotate(-45deg);
  }
}
.home {
  position: relative;
  display: flex;
  flex-direction: column; /* Change to column layout */
  justify-content: center;
  align-items: center;
  height: 100vh;
  background-color: #121212;
  color: #e0e0e0;
  text-align: center;
  overflow: hidden;
}

.title-container {
  position: relative;
  display: inline-block;
  margin-bottom: 20px;
}

#tsparticles {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: 0;
}

.content {
  position: relative;
  z-index: 2; /* Ensure content is on top */
  transform: translateY(-5%);
}

.name {
  font-family: 'Rajdhani', sans-serif;
  font-size: 9rem;
  font-weight: 700;
  margin: 0;
  letter-spacing: 0.1em;
  background: linear-gradient(to right, #4a00e0, #8e2de2, #ffd700, #8e2de2, #4a00e0);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-size: 200% auto;
  animation: gradient-scroll 3s linear infinite;
  transform: skewX(-15deg);
  -webkit-box-reflect: below 0px linear-gradient(to bottom, rgba(0,0,0,0.0), rgba(0,0,0,0.4));
  position: relative;
  z-index: 1;
}

@keyframes gradient-scroll {
  from {
    background-position: 0% center;
  }
  to {
    background-position: -200% center;
  }
}

.subtitle {
  font-size: 1.5rem;
  margin: 10px 0;
}

.tagline {
  font-size: 1rem;
  color: #a0a0a0;
  margin-bottom: 4rem;
}

.welcome {
  font-size: 2.5rem;
  font-weight: 500;
  margin: 20px 0;
}

.description {
  font-size: 1.2rem;
  color: #b0b0b0;
}
</style>