<script setup>
import { RouterView } from 'vue-router'
import Footer from './components/Footer.vue'
</script>

<template>
  <div class="app-container">
    <RouterView />
    <Footer />
  </div>
</template>

<style>
/* 全局样式 */
:root {
  --color-bg-start: #fde7f0;
  --color-bg-end: #e6e9ff;
  --color-primary: #ff6b81;
  --color-text-dark: #3D3B56;
  --color-text-light: #6a6882;
}

body, html {
  margin: 0;
  padding: 0;
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif;
  height: 100%;
}

#app {
  height: 100%;
}

.app-container {
  display: flex;
  flex-direction: column;
  min-height: 100%;
}

/* 移动端适配 */
@media (max-width: 768px) {
  html {
    font-size: 14px;
  }
}

@media (max-width: 480px) {
  html {
    font-size: 12px;
  }
}
</style>