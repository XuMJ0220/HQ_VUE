<template>
  <nav class="navbar">
    <div class="navbar-logo">
      <a href="#">XuMJ</a>
    </div>
    <ul class="navbar-links">
      <li><a href="#">首页</a></li>
      <li><a href="#">关于</a></li>
      <li><a href="#">项目</a></li>
      <li><a href="#">联系我</a></li>
    </ul>
    <div class="navbar-auth">
      <button class="btn-login">登录</button>
      <router-link to="/register" class="btn-signup">注册</router-link>
    </div>
  </nav>
</template>

<script>
export default {
  name: 'Navbar'
}
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
  color: #f0c419;
}

.navbar-auth .btn-login,
.navbar-auth .btn-signup {
  display: inline-block;
  padding: 0.6rem 1.2rem;
  border: 1px solid #f0c419;
  border-radius: 25px;
  background-color: transparent;
  color: #f0c419;
  text-decoration: none;
  cursor: pointer;
  transition: all 0.3s ease;
  font-family: inherit;
  font-size: 0.95em;
  font-weight: 500;
  letter-spacing: 0.5px;
}

.navbar-auth .btn-login {
  margin-right: 0.8rem;
}

.navbar-auth .btn-login:hover,
.navbar-auth .btn-signup:hover {
  background-color: #f0c419;
  color: #1a1a1a;
  box-shadow: 0 0 15px rgba(240, 196, 25, 0.5);
  transform: translateY(-2px);
}
</style>