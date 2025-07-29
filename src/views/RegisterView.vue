<template>
  <div class="register-wrapper">
    <Particles
      id="tsparticles"
      :particlesInit="particlesInit"
      :particlesLoaded="particlesLoaded"
      :options="particleOptions"
    />
    <div class="register-container">
      <h2>创建账户</h2>
      <p class="subtitle">加入我们, 开始您的旅程</p>
      <form @submit.prevent="handleRegister">
        <div class="form-group">
          <input type="text" id="username" v-model="username" placeholder="请输入用户名" required>
        </div>
        <div class="form-group">
          <input type="email" id="email" v-model="email" placeholder="请输入邮箱地址" required>
        </div>
        <div class="form-group">
          <input type="password" id="password" v-model="password" placeholder="请输入密码" required>
        </div>
        <div class="form-group">
          <input type="password" id="re_password" v-model="re_password" placeholder="请再次输入密码" required>
        </div>
        <div class="form-group">
          <select id="gender" v-model="gender" required>
            <option value="1">男</option>
            <option value="2">女</option>
            <option value="0">未知</option>
          </select>
        </div>
        <button type="submit">注册</button>
      </form>
      <div class="links">
        <p>已有账户? <router-link to="/login">立即登录</router-link></p>
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
      email: '',
      password: '',
      re_password: '',
      gender: 1, // 默认性别为男
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
    handleRegister() {
      if (this.password !== this.re_password) {
        alert('两次输入的密码不一致');
        return;
      }

      axios.post('/api/v1/user/signup', {
        username: this.username,
        password: this.password,
        re_password: this.re_password,
        email: this.email,
        gender: parseInt(this.gender),
      })
      .then(response => {
        if (response.data.code === 1004) {
          alert('注册成功');
          this.$router.push('/'); // 注册成功后跳转到首页
        } else {
          alert('注册失败: ' + response.data.msg);
        }
      })
      .catch(error => {
        console.error('注册失败:', error);
        alert('注册失败，请稍后再试');
      });
    },
  },
};
</script>

<style scoped>
.register-wrapper {
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

.register-container {
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
  border: 1px solid rgba(255, 255, 255, 0.1);
  text-align: center;
}

h2 {
  font-size: 2.5rem;
  font-weight: bold;
  color: #7f5af0;
  margin-bottom: 0.5rem;
}

.subtitle {
  margin-bottom: 2rem;
  color: #ccc;
}

.form-group {
  margin-bottom: 1.5rem;
  text-align: left;
}

.form-group input,
.form-group select {
  width: 100%;
  padding: 12px 15px;
  background-color: rgba(255, 255, 255, 0.1);
  border: 1px solid rgba(255, 255, 255, 0.2);
  border-radius: 8px;
  color: #ffffff; /* 输入时文字颜色 */
  font-size: 1rem;
  transition: all 0.3s ease;
  box-sizing: border-box;
}

.form-group input:focus,
.form-group select:focus {
  outline: none;
  background-color: rgba(255, 255, 255, 0.95); /* 激活时背景变白 */
  color: #1a1a1a; /* 激活时文字变黑 */
  border-color: #7f5af0;
  box-shadow: 0 0 8px rgba(127, 90, 240, 0.5);
}

.form-group input::placeholder {
  color: #a0a0a0; /* 占位符颜色 */
}

button[type="submit"] {
  width: 100%;
  padding: 12px;
  background: linear-gradient(90deg, #7f5af0, #a77bf3);
  border: none;
  border-radius: 8px;
  color: white;
  font-size: 1.1rem;
  font-weight: bold;
  cursor: pointer;
  transition: all 0.3s ease;
  margin-top: 1rem;
}

button[type="submit"]:hover {
  box-shadow: 0 0 15px rgba(127, 90, 240, 0.8);
  transform: translateY(-2px);
}

.links {
  margin-top: 1.5rem;
}

.links a {
  color: #7f5af0;
  text-decoration: none;
  transition: color 0.3s ease;
}

.links a:hover {
  color: #a77bf3;
}
</style>