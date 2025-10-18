<script setup>
import { ref, computed, watch } from 'vue'
import { useRouter } from 'vue-router'
import { quizQuestions } from '../quizData.js'
import { useParticleAnimation } from '../composables/useParticleAnimation'

const router = useRouter()
const canvasRef = ref(null)

useParticleAnimation(canvasRef)

const currentQuestionIndex = ref(0)
const answerHistory = ref([]) // 记录每道题选择的选项索引
const scoresHistory = ref([]) // 记录每道题获得的分数
const selectedOption = ref(null) // 当前题选择的选项
const isCompleted = ref(false)

const currentQuestion = computed(() => quizQuestions[currentQuestionIndex.value])
const progress = computed(() => ((currentQuestionIndex.value) / quizQuestions.length) * 100)

// 监听题目变化，更新selectedOption
watch(currentQuestionIndex, (newIndex) => {
  selectedOption.value = answerHistory.value[newIndex] !== undefined ? answerHistory.value[newIndex] : null;
});

function selectAnswer(option, index) {
  if (selectedOption.value === index) return; // 防止重复点击
  
  selectedOption.value = index;
  answerHistory.value[currentQuestionIndex.value] = index;
  scoresHistory.value[currentQuestionIndex.value] = option.scores;

  // 选中答案后延迟400ms自动跳转到下一题
  setTimeout(() => {
    nextQuestion();
  }, 250);
}

function nextQuestion() {
  if (selectedOption.value === null && answerHistory.value[currentQuestionIndex.value] === undefined) return;
  
  if (currentQuestionIndex.value < quizQuestions.length - 1) {
    currentQuestionIndex.value++;
  } else {
    isCompleted.value = true;
  }
}

function prevQuestion() {
  if (currentQuestionIndex.value > 0) {
    currentQuestionIndex.value--;
  }
}

function calculateAndGoToResults() {
  // 扩展为六维人格
  const finalScores = { intimacy: 0, expression: 0, stability: 0, empathy: 0, autonomy: 0, growth: 0 };
  scoresHistory.value.forEach(scoreObj => {
    if (scoreObj) {
        for (const dim in scoreObj) {
            finalScores[dim] += scoreObj[dim];
        }
    }
  });

  router.push({ 
    name: 'result', 
    query: { ...finalScores } 
  });
}
</script>

<template>
  <div class="quiz-container">
    <canvas ref="canvasRef" class="particle-canvas"></canvas>
    
    <header class="quiz-header">
      <div class="progress-bar-container">
        <div class="progress-bar" :style="{ width: progress + '%' }"></div>
      </div>
      <h2 class="question-counter" v-if="!isCompleted">Question {{ currentQuestionIndex + 1 }} of {{ quizQuestions.length }}</h2>
    </header>

    <main class="card-container">
      <Transition name="slide-up" mode="out-in">
        <div v-if="!isCompleted" class="card" :key="currentQuestionIndex">
          <p class="question-text">{{ currentQuestion.text }}</p>
          <div class="options-container">
            <button
              v-for="(option, index) in currentQuestion.options"
              :key="index"
              class="option-button"
              :class="{ 'selected': selectedOption === index }"
              @click="selectAnswer(option, index)"
            >
              {{ option.text }}
            </button>
          </div>
          <div class="navigation-buttons">
            <button @click="prevQuestion" :disabled="currentQuestionIndex === 0" class="nav-button prev-button">
              <span>&larr;</span> 上一题
            </button>
             <button @click="nextQuestion" class="nav-button next-button" v-if="currentQuestionIndex === quizQuestions.length - 1 && !isCompleted" :disabled="selectedOption === null">
              完成 <span>&rarr;</span>
            </button>
          </div>
        </div>
        <div v-else class="card completion-card">
            <h2 class="completion-title">🎉 恭喜你！</h2>
            <p class="completion-text">已完成所有灵魂问答，你的专属恋爱人格报告已生成。</p>
            <button @click="calculateAndGoToResults" class="result-button">查看分析结果</button>
        </div>
      </Transition>
    </main>
  </div>
</template>

<style scoped>
.quiz-container {
  display: flex; flex-direction: column; align-items: center;
  min-height: 100vh; background: linear-gradient(160deg, #fdf4f6, #e7e9fc);
  overflow: hidden; position: relative;
  font-size: 1.1rem; /* 增大基础字体 */
}
.particle-canvas {
  position: absolute; top: 0; left: 0; width: 100%; height: 100%; z-index: 1;
}
.quiz-header, .card-container { z-index: 2; }
.quiz-header {
  width: 100%; max-width: 550px; position: fixed; top: 20px;
  left: 50%; transform: translateX(-50%); padding: 0 20px;
}
.progress-bar-container {
  width: 100%; height: 8px; background-color: rgba(0, 0, 0, 0.08);
  border-radius: 4px; overflow: hidden;
}
.progress-bar {
  height: 100%; background-color: #ff8fab; border-radius: 4px;
  transition: width 0.6s cubic-bezier(0.65, 0, 0.35, 1);
}
.question-counter {
  text-align: center; margin-top: 1rem; font-size: 1rem;
  font-weight: 500; color: #8c82a3;
}
.card-container {
  flex-grow: 1; display: flex; align-items: center; justify-content: center;
  width: 100%; padding: 120px 20px 50px 20px;
}
.card {
  width: 100%; max-width: 550px; padding: 3rem;
  background: rgba(255, 255, 255, 0.75); backdrop-filter: blur(25px);
  border-radius: 28px; border: 1px solid rgba(255, 255, 255, 0.3);
  box-shadow: 0 20px 50px rgba(0,0,0,0.12);
}
.question-text {
  font-size: 1.8rem; font-weight: 600; color: #3D3B56;
  margin-bottom: 3rem; text-align: center; line-height: 1.6;
}
.options-container {
  display: grid; grid-template-columns: 1fr; gap: 1.2rem;
}
.option-button {
  width: 100%; padding: 1.2rem; font-size: 1.1rem; font-family: inherit;
  text-align: left; line-height: 1.6; background-color: #ffffff;
  color: #4A4A6A; border: 2px solid #e0ddee; border-radius: 16px;
  cursor: pointer; transition: all 0.3s ease;
}
.option-button:hover {
  transform: translateY(-5px); box-shadow: 0 10px 25px rgba(0,0,0,0.08);
  border-color: #ffadc8;
}
.option-button.selected {
  border-color: #ff6b81;
  background-color: #fff5f7;
  color: #d63031;
  font-weight: 500;
  box-shadow: 0 6px 20px rgba(255, 107, 129, 0.2);
}
.navigation-buttons {
    display: flex; justify-content: space-between; align-items: center; margin-top: 2.5rem;
}
.nav-button {
    display: flex; align-items: center; gap: 0.5rem;
    background: transparent; border: none; font-size: 1.1rem;
    font-weight: 500; color: #8c82a3; padding: 0.5rem 1rem;
    border-radius: 10px; cursor: pointer; transition: all 0.3s ease;
}
.nav-button.prev-button:disabled {
    opacity: 0.4; cursor: not-allowed;
}
.nav-button.prev-button:not(:disabled):hover {
    background-color: #f0eefc;
}
.nav-button.next-button {
    color: white; background-color: #ff8fab;
    padding: 0.9rem 1.8rem; border-radius: 14px;
}
.nav-button.next-button:disabled {
    background-color: #d1c9e0; cursor: not-allowed;
}
.nav-button.next-button:not(:disabled):hover {
    background-color: #ff6b81;
    transform: translateY(-3px);
    box-shadow: 0 8px 25px rgba(255, 107, 129, 0.4);
}
.completion-card { text-align: center; }
.completion-title {
    font-size: 2.8rem; font-weight: 700; color: #3D3B56; margin-bottom: 1.5rem;
}
.completion-text {
    font-size: 1.2rem; color: #6a6882; line-height: 1.8; margin-bottom: 3rem;
}
.result-button {
    padding: 1.2rem 2.5rem; font-size: 1.3rem; font-weight: bold;
    color: white; background-color: #ff6b81; border: none;
    border-radius: 18px; cursor: pointer; transition: all 0.3s ease;
}
.result-button:hover {
    transform: translateY(-6px);
    box-shadow: 0 10px 30px rgba(255, 107, 129, 0.5);
}
.slide-up-enter-active, .slide-up-leave-active {
  transition: all 0.6s cubic-bezier(0.65, 0, 0.35, 1);
}
.slide-up-enter-from { opacity: 0; transform: translateY(60px); }
.slide-up-leave-to { opacity: 0; transform: translateY(-60px); }
</style>
