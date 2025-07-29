<template>
  <nav class="navbar">
    <div class="navbar-logo">
      <a href="#">XuMJ</a>
    </div>
    <ul class="navbar-links">
      <li><router-link to="/">首页</router-link></li>
      <li><a href="#">关于</a></li>
      <li><a href="#">项目</a></li>
      <li><a href="#">联系我</a></li>
    </ul>
    <div class="navbar-auth">
      <template v-if="!isLoggedIn">
        <router-link to="/login" class="btn-login">登录</router-link>
        <router-link to="/register" class="btn-signup">注册</router-link>
      </template>
      <template v-else>
        <div class="user-profile" ref="userProfile" @click="toggleDropdown">
          <img :src="avatarUrl" alt="User Avatar" class="avatar">
          <div v-if="dropdownOpen" class="dropdown-menu">
            <a href="#" class="dropdown-item">
              <span v-if="role === '管理员'">管理员：</span>
              <span v-else>用户：</span>
              {{ username }}
            </a>
            <template v-if="role === '管理员'">
              <div class="dropdown-divider"></div>
              <router-link to="/admin" class="dropdown-item">进入后台管理</router-link>
            </template>
            <div class="dropdown-divider"></div>
            <a href="#" @click.prevent="logout" class="dropdown-item">退出</a>
          </div>
        </div>
      </template>
    </div>
  </nav>
</template>

<script setup>
import { ref, onMounted, onBeforeUnmount, watch } from 'vue';
import { useRouter, useRoute } from 'vue-router';
import defaultAvatar from '@/assets/default_avatar.webp';

const router = useRouter();
const route = useRoute();

const isLoggedIn = ref(false);
const username = ref('');
const role = ref('');
const avatarUrl = ref(defaultAvatar);
const dropdownOpen = ref(false);
const userProfile = ref(null);

const checkLoginStatus = () => {
  const token = localStorage.getItem('token');
  if (token) {
    isLoggedIn.value = true;
    username.value = localStorage.getItem('username') || '用户';
    role.value = localStorage.getItem('role');
  } else {
    isLoggedIn.value = false;
    username.value = '';
  }
};

const logout = () => {
  localStorage.removeItem('token');
  localStorage.removeItem('role');
  localStorage.removeItem('username');
  isLoggedIn.value = false;
  username.value = '';
  dropdownOpen.value = false;
  router.push('/');
};

const toggleDropdown = () => {
  dropdownOpen.value = !dropdownOpen.value;
};

const closeDropdownOnClickOutside = (event) => {
  if (userProfile.value && !userProfile.value.contains(event.target)) {
    dropdownOpen.value = false;
  }
};

onMounted(() => {
  checkLoginStatus();
  document.addEventListener('click', closeDropdownOnClickOutside);
});

onBeforeUnmount(() => {
  document.removeEventListener('click', closeDropdownOnClickOutside);
});

watch(() => route.path, () => {
  checkLoginStatus();
});
</script>

<style scoped>
.navbar {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 0.5rem 2rem;
  background-color: rgba(26, 26, 26, 0.8);
  color: #fff;
  position: fixed;
  width: 100%;
  top: 0;
  left: 0;
  z-index: 1000;
  backdrop-filter: blur(10px);
}

.navbar-logo a {
  color: #fff;
  text-decoration: none;
  font-size: 1.5rem;
  font-weight: bold;
}

.navbar-links {
  list-style: none;
  display: flex;
  gap: 1.5rem;
}

.navbar-links a {
  color: #fff;
  text-decoration: none;
  transition: color 0.3s ease;
}

.navbar-links a:hover {
  color: #00aaff;
}

.navbar-auth .btn-login,
.navbar-auth .btn-signup {
  display: inline-block;
  padding: 0.6rem 1.2rem;
  border: 1px solid #00aaff;
  border-radius: 25px;
  background-color: transparent;
  color: #00aaff;
  text-decoration: none;
  cursor: pointer;
  transition: all 0.3s ease;
  font-family: inherit;
  font-size: 0.95em;
  font-weight: 500;
  letter-spacing: 0.5px;
}

.navbar-auth .btn-login, .navbar-auth .welcome-message {
  margin-right: 0.8rem;
}

.welcome-message {
  color: #fff;
  font-weight: 500;
}

.navbar-auth .btn-login,
.navbar-auth .btn-signup,
.navbar-auth .btn-logout {
  display: inline-block;
  padding: 0.6rem 1.2rem;
  border: 1px solid #00aaff;
  border-radius: 25px;
  background-color: transparent;
  color: #00aaff;
  text-decoration: none;
  cursor: pointer;
  transition: all 0.3s ease;
  font-family: inherit;
  font-size: 0.95em;
  font-weight: 500;
  letter-spacing: 0.5px;
}

.navbar-auth .btn-login:hover,
.navbar-auth .btn-signup:hover,
.navbar-auth .btn-logout:hover {
  background-color: #00aaff;
  color: #fff;
  box-shadow: 0 0 15px rgba(0, 170, 255, 0.5);
  transform: translateY(-2px);
}

.user-profile {
  position: relative;
  cursor: pointer;
}

.avatar {
  width: 40px;
  height: 40px;
  border-radius: 50%;
  border: 2px solid #00aaff;
}

.dropdown-menu {
  position: absolute;
  top: 55px;
  right: 0;
  background-color: #2c2c2c;
  border-radius: 8px;
  box-shadow: 0 8px 30px rgba(0, 0, 0, 0.3);
  width: 200px;
  padding: 0.5rem 0;
  overflow: hidden;
  animation: dropdown-fade-in 0.3s ease-out;
  width: 200px;
  z-index: 1100;
  backdrop-filter: blur(10px);
  border: 1px solid rgba(255, 255, 255, 0.1);
}

.dropdown-item {
  display: block;
  padding: 12px 20px;
  color: #fff;
  text-decoration: none;
  transition: background-color 0.2s ease;
  font-size: 0.95rem;
}

.dropdown-item:first-child {
  border-top-left-radius: 8px;
  border-top-right-radius: 8px;
}

.dropdown-item:last-child {
  border-bottom-left-radius: 8px;
  border-bottom-right-radius: 8px;
}

.dropdown-item:hover {
    background-color: #555;
    color: #fff;
   }

.dropdown-divider {
  height: 1px;
  background-color: rgba(255, 255, 255, 0.1);
  margin: 5px 0;
}
</style>