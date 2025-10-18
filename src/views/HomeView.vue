<script setup>
import { ref } from 'vue'
import { useRouter } from 'vue-router'
import { useParticleAnimation } from '../composables/useParticleAnimation' // 引入动画模块

const router = useRouter()
const canvasRef = ref(null)

// 一行代码即可调用粒子动画！
useParticleAnimation(canvasRef)

function startQuiz() {
  router.push('/quiz')
}
</script>

<template>
  <div class="home-container">
    <canvas ref="canvasRef" class="particle-canvas"></canvas>
    <main class="content-wrapper">
      <div class="badge">
        <div class="dashed-circle"></div>
        <svg class="heart-icon" viewBox="0 0 24 24">
          <path fill="currentColor" d="M12,21.35L10.55,20.03C5.4,15.36 2,12.27 2,8.5C2,5.41 4.42,3 7.5,3C9.24,3 10.91,3.81 12,5.08C13.09,3.81 14.76,3 16.5,3C19.58,3 22,5.41 22,8.5C22,12.27 18.6,15.36 13.45,20.03L12,21.35Z" />
        </svg>
      </div>
      <h1 class="main-title">你的恋爱人格是什么？</h1>
      <p class="subtitle">16道题揭秘你的专属恋爱模式 ✨</p>
      <p class="guide-text">准备好发现真实的自己了吗？</p>
      <button @click="startQuiz" class="start-button">
        <span>🚀 开始探索</span>
      </button>
    </main>
    <footer class="footer">
      <div class="divider"></div>
      <p>你的心，藏着哪颗闪亮的星？</p>
      <p class="brand">小红书 @ 元认知星图</p>
    </footer>
  </div>
</template>

<style scoped>
/* 样式代码和之前版本完全一样，这里省略，请保留你文件里已有的样式 */
:root {
  --color-bg-start: #fde7f0;
  --color-bg-end: #e6e9ff;
  --color-primary: #ff6b81;
  --color-text-dark: #3D3B56;
  --color-text-light: #6a6882;
}

.home-container {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
  padding: 20px;
  background: linear-gradient(135deg, var(--color-bg-start), var(--color-bg-end));
  overflow: hidden;
  position: relative;
}

.particle-canvas {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: 1;
}

.content-wrapper {
  display: flex;
  flex-direction: column;
  align-items: center;
  text-align: center;
  animation: fadeIn 1s ease-out;
  z-index: 2;
  flex-grow: 1;
  justify-content: center;
}

@keyframes fadeIn {
  from { opacity: 0; transform: translateY(20px); }
  to { opacity: 1; transform: translateY(0); }
}

.badge {
  position: relative;
  width: 130px;
  height: 130px;
  display: flex;
  justify-content: center;
  align-items: center;
  margin-bottom: 3rem;
}
.dashed-circle {
  position: absolute;
  width: 100%;
  height: 100%;
  border: 3px dashed var(--color-primary);
  opacity: 0.8;
  border-radius: 50%;
  animation: rotate 30s linear infinite;
}
@keyframes rotate {
  from { transform: rotate(0deg); }
  to { transform: rotate(360deg); }
}
.heart-icon {
  width: 70px;
  height: 70px;
  color: var(--color-primary);
  animation: pulse 2s infinite ease-in-out;
  filter: drop-shadow(0 5px 10px rgba(255, 107, 129, 0.6));
}
@keyframes pulse {
  0%, 100% { transform: scale(1); }
  50% { transform: scale(1.15); }
}

.main-title {
  font-family: 'PingFang SC', 'Microsoft YaHei', sans-serif;
  font-size: 3rem;
  color: var(--color-text-dark);
  font-weight: 700;
  margin: 0;
  line-height: 1.2;
}
.subtitle {
  font-size: 1.2rem;
  color: var(--color-text-light);
  margin-top: 1rem;
}
.guide-text {
  font-size: 1.1rem;
  color: var(--color-text-light);
  margin-top: 3rem;
  margin-bottom: 2rem;
}

.start-button {
  background-color: white;
  color: var(--color-primary);
  border: none;
  border-radius: 50px;
  padding: 20px 45px;
  font-size: 1.3rem;
  font-weight: bold;
  cursor: pointer;
  transition: all 0.3s ease;
  box-shadow: 0 8px 30px rgba(255, 107, 129, 0.4);
}
.start-button:hover {
  transform: translateY(-6px);
  box-shadow: 0 12px 35px rgba(255, 107, 129, 0.6);
}
.start-button:active {
  transform: translateY(0);
  box-shadow: 0 8px 30px rgba(255, 107, 129, 0.4);
}

.footer {
  width: 100%;
  text-align: center;
  color: var(--color-text-light);
  font-size: 0.9rem;
  padding-bottom: 1.5rem;
  z-index: 2;
}
.footer p {
  margin: 8px 0;
}
.footer .brand {
  font-weight: 600;
  color: var(--color-text-dark);
}
.divider {
  width: 60px;
  height: 1px;
  background-color: var(--color-text-light);
  margin: 0 auto 15px;
  opacity: 0.5;
}
</style>