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
      <button class="btn-signup">注册</button>
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
  padding: 1rem 2rem;
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

.navbar-auth button {
  border: none;
  padding: 0.5rem 1rem;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.btn-login {
  background-color: transparent;
  color: #fff;
  border: 1px solid #fff;
  margin-right: 0.5rem;
}

.btn-login:hover {
  background-color: #fff;
  color: #1a1a1a;
}

.btn-signup {
  background-color: transparent;
  color: #fff;
  border: 1px solid #fff;
}

.btn-signup:hover {
  background-color: #fff;
  color: #1a1a1a;
}
</style>