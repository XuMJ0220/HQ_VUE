<template>
  <div id="app-container">
    <Navbar />
    <RouterView />
  </div>
</template>

<script setup>
import { RouterView } from 'vue-router'
import Navbar from './components/Navbar.vue'
</script>

<style scoped>
#app-container {
  height: 100%;
  width: 100%;
}
</style>