<template>
  <div class="login-wrapper">
    <Particles
      id="tsparticles"
      :particlesInit="particlesInit"
      :particlesLoaded="particlesLoaded"
      :options="particleOptions"
    />
    <div class="login-container">
      <h2>登录您的账户</h2>
      <p class="subtitle">欢迎回来！</p>
      <form @submit.prevent="handleLogin">
        <div class="form-group">
          <input type="text" id="username" v-model="username" placeholder="请输入用户名" required>
        </div>
        <div class="form-group">
          <input type="password" id="password" v-model="password" placeholder="请输入密码" required>
        </div>
        <button type="submit">登录</button>
      </form>
      <div class="links">
        <p>还没有账户? <router-link to="/register">立即注册</router-link></p>
        <router-link to="/">返回首页</router-link>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import { loadFull } from "tsparticles";

export default {
  data() {
    return {
      username: '',
      password: '',
      particleOptions: {
        background: {
          color: {
            value: 'transparent'
          }
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
            bubble: {
              distance: 400,
              duration: 2,
              opacity: 0.8,
              size: 40
            },
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
            outMode: 'bounce',
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
            random: true,
            value: 5
          }
        },
        detectRetina: true
      }
    };
  },
  methods: {
    async particlesInit(engine) {
      await loadFull(engine);
    },
    async particlesLoaded(container) {
      console.log("Particles container loaded", container);
    },
    handleLogin() {
      axios.post('/api/v1/user/login', {
        username: this.username,
        password: this.password,
      })
      .then(response => {
        if (response.data.code === 1006 || response.data.code === 1002) {
          alert('登录成功');
          // 保存token到localStorage
          localStorage.setItem('token', response.data.detail.token);
          localStorage.setItem('role', response.data.detail.role);
          localStorage.setItem('username', response.data.detail.user_name);
          this.$router.push('/'); // 登录成功后跳转到首页
        } else {
          alert('登录失败: ' + response.data.msg);
        }
      })
      .catch(error => {
        console.error('登录失败:', error);
        alert('登录失败，请稍后再试');
      });
    },
  },
};
</script>

<style scoped>
.login-wrapper {
  position: relative;
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
  padding: 2rem;
  overflow: hidden;
}

#tsparticles {
  position: absolute;
  width: 100%;
  height: 100%;
  top: 0;
  left: 0;
  z-index: 0;
}

.login-container {
  position: relative;
  z-index: 1;
  width: 100%;
  max-width: 420px;
  padding: 3rem;
  background: rgba(25, 25, 25, 0.8);
  border-radius: 25px;
  box-shadow: 0 12px 40px rgba(0, 0, 0, 0.5);
  backdrop-filter: blur(12px);
  color: #e0e0e0;
  text-align: center;
}

h2 {
  margin-bottom: 1rem;
  font-size: 2.5rem;
  font-weight: 700;
  color: #fff;
  text-shadow: 0 2px 4px rgba(0,0,0,0.2);
}

.subtitle {
  margin-bottom: 2rem;
  font-size: 1.1rem;
  color: #ccc;
}

.form-group {
  margin-bottom: 1.5rem;
}

input[type="text"],
input[type="password"] {
  width: 100%;
  padding: 1rem;
  font-size: 1rem;
  border: 1px solid #444;
  border-radius: 8px;
  background-color: rgba(0, 0, 0, 0.3);
  color: #fff;
  transition: all 0.3s ease;
}

input[type="text"]:focus,
input[type="password"]:focus {
  outline: none;
  background-color: #4a4a4a;
  color: #ffffff;
  border-color: #6c6c6c;
  box-shadow: 0 0 8px rgba(108, 108, 108, 0.5);
}

button[type="submit"] {
  width: 100%;
  padding: 1rem;
  font-size: 1.2rem;
  font-weight: 600;
  color: #fff;
  background: linear-gradient(45deg, #4a90e2, #50e3c2);
  border: none;
  border-radius: 8px;
  cursor: pointer;
  transition: all 0.3s ease;
  box-shadow: 0 4px 15px rgba(0, 0, 0, 0.2);
}

button[type="submit"]:hover {
  transform: translateY(-2px);
  box-shadow: 0 6px 20px rgba(74, 144, 226, 0.4);
}

.links {
  margin-top: 2rem;
}

.links p {
  margin-bottom: 1rem;
}

.links a {
  color: #4a90e2;
  text-decoration: none;
  transition: color 0.3s ease;
}

.links a:hover {
  color: #50e3c2;
  text-decoration: underline;
}
</style>