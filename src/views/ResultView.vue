<script setup>
import { ref, onMounted, computed } from 'vue'
import { useRoute, useRouter } from 'vue-router'
import Chart from 'chart.js/auto'
import { resultsData } from '../analysisData/index.js'
import { dimensionMapping } from '../quizData.js'

const route = useRoute()
const router = useRouter()

// 升级为六维人格
const scores = {
  intimacy: Number(route.query.intimacy || 0),
  expression: Number(route.query.expression || 0),
  stability: Number(route.query.stability || 0),
  empathy: Number(route.query.empathy || 0),
  autonomy: Number(route.query.autonomy || 0),
  growth: Number(route.query.growth || 0),
};

const sortedDimensions = computed(() => {
  return Object.entries(scores).sort(([, a], [, b]) => b - a);
});

const mainDimension = computed(() => sortedDimensions.value[0][0]);
const subDimension = computed(() => sortedDimensions.value[1][0]);

const mainAnimal = computed(() => dimensionMapping[mainDimension.value]);
const subAnimal = computed(() => dimensionMapping[subDimension.value]);

const result = computed(() => {
  const mainData = resultsData[mainAnimal.value];
  const combinationData = mainData.combinations[subAnimal.value] || {
    subDescription: '你内在的特质，形成了一种独特的组合。',
    chemistry: { title: '独特的化学反应', text: '你的主导人格与辅助人格形成了一种独特的组合，这让你在关系中展现出复杂而迷人的特质。'},
    strengths: ['独特', '复杂', '有魅力'],
    challenges: ['矛盾', '难以预测', '需整合'],
    partner: '你需要一个能理解并欣赏你多面性的伴侣。',
    proverb: '认识自己，是所有智慧的开端。',
  };
  return { ...mainData, ...combinationData };
});

const themeVars = computed(() => result.value.theme);
const chartRef = ref(null);
const barChartRef = ref(null);

// 增加恋爱风格
const styleScores = computed(() => {
    const logic = scores.stability + scores.growth - scores.intimacy - scores.empathy;
    const proactive = scores.expression + scores.autonomy - scores.empathy;
    const optimistic = scores.expression + scores.intimacy - scores.stability;
    const romantic = scores.intimacy + scores.expression - scores.stability - scores.growth;
    const direct = scores.expression + scores.stability - scores.empathy - scores.autonomy;
    const invested = scores.intimacy + scores.stability + scores.growth - scores.autonomy;
    const tolerant = scores.empathy + scores.intimacy - scores.expression;
    return {
        rational: Math.max(0, Math.min(100, 50 + logic * 4)),
        proactive: Math.max(0, Math.min(100, 50 + proactive * 4)),
        optimistic: Math.max(0, Math.min(100, 50 + optimistic * 4)),
        romantic: Math.max(0, Math.min(100, 50 + romantic * 4)),
        direct: Math.max(0, Math.min(100, 50 + direct * 4)),
        invested: Math.max(0, Math.min(100, 50 + invested * 4)),
        tolerant: Math.max(0, Math.min(100, 50 + tolerant * 4)),
    }
});


onMounted(() => {
  // 六维雷达图
  if (chartRef.value && themeVars.value) {
    const ctx = chartRef.value.getContext('2d');
    const chartData = [scores.intimacy, scores.expression, scores.stability, scores.empathy, scores.autonomy, scores.growth];
    const maxScore = Math.max(...chartData, 8);
    new Chart(ctx, {
      type: 'radar',
      data: {
        labels: ['亲密驱动', '表达风格', '稳定需求', '共情能力', '自主需求', '成长驱动'],
        datasets: [{
          data: chartData,
          backgroundColor: colorWithAlpha(themeVars.value['--theme-primary'], 0.2),
          borderColor: themeVars.value['--theme-primary'],
          borderWidth: 3,
          pointBackgroundColor: themeVars.value['--theme-primary'],
          pointRadius: 6,
          pointHoverRadius: 8,
        }]
      },
      options: {
        plugins: { legend: { display: false } },
        scales: {
          r: {
            angleLines: { color: 'rgba(0, 0, 0, 0.08)' },
            grid: { color: 'rgba(0, 0, 0, 0.08)' },
            pointLabels: { font: { size: 16, weight: '500' }, color: '#3D3B56' },
            ticks: { display: false, beginAtZero: true, max: maxScore + 2 }
          }
        },
        maintainAspectRatio: false,
      }
    });
  }

  // 恋爱风格条形图
  if (barChartRef.value && themeVars.value) {
      const ctx = barChartRef.value.getContext('2d');
      new Chart(ctx, {
          type: 'bar',
          data: {
              labels: ['理性 / 感性', '主动 / 被动', '乐观 / 审慎', '浪漫 / 务实', '直接 / 委婉', '投入 / 抽离', '包容 / 挑剔'],
              datasets: [{
                  data: [
                    styleScores.value.rational, styleScores.value.proactive, styleScores.value.optimistic, 
                    styleScores.value.romantic, styleScores.value.direct, styleScores.value.invested, 
                    styleScores.value.tolerant
                  ],
                  backgroundColor: colorWithAlpha(themeVars.value['--theme-primary'], 0.6),
                  borderRadius: 8,
                  borderSkipped: false,
              }]
          },
          options: {
              indexAxis: 'y',
              plugins: { legend: { display: false }, tooltip: { enabled: false } },
              scales: {
                  x: { min: 0, max: 100, grid: { display: false }, ticks: { display: false } },
                  y: { grid: { display: false }, ticks: { font: { size: 16, weight: '500' }, color: '#3D3B56' } }
              },
          }
      });
  }
});

function retest() {
  router.push('/');
}

function colorWithAlpha(hex, alpha) {
    if (!hex) return `rgba(255, 107, 129, ${alpha})`;
    const r = parseInt(hex.slice(1, 3), 16);
    const g = parseInt(hex.slice(3, 5), 16);
    const b = parseInt(hex.slice(5, 7), 16);
    return `rgba(${r}, ${g}, ${b}, ${alpha})`;
}
</script>

<template>
  <div class="result-container" :style="themeVars">
    <div class="card">
      <header class="card-header">
        <img :src="result.image" :alt="result.name" class="animal-image">
        <h3 class="result-type">你的恋爱人格是</h3>
        <h1 class="animal-name">{{ result.name }}</h1>
        <p class="animal-title">{{ result.title }}</p>
      </header>
      
      <section class="result-section">
        <p class="description" v-html="result.description"></p>
        <p class="description sub-description" v-html="result.subDescription"></p>
      </section>

      <section class="result-section">
        <h2 class="section-title"><span class="icon">🔮</span> 灵魂深处的化学反应</h2>
        <div class="chemistry-content">
          <h4 class="chemistry-title">{{ result.chemistry.title }}</h4>
          <p v-html="result.chemistry.text"></p>
        </div>
      </section>
      
      <div class="strengths-challenges-grid">
        <section class="result-section">
          <h2 class="section-title"><span class="icon">✨</span> 你的天赋与光辉</h2>
          <ul class="custom-list">
            <li v-for="item in result.strengths" :key="item" v-html="item"></li>
          </ul>
        </section>

        <section class="result-section">
          <h2 class="section-title"><span class="icon">🌗</span> 需觉察的内在阴影</h2>
          <ul class="custom-list">
            <li v-for="item in result.challenges" :key="item" v-html="item"></li>
          </ul>
        </section>
      </div>

       <section class="result-section">
        <h2 class="section-title"><span class="icon">💞</span> 灵魂共鸣指引</h2>
        <p v-html="result.partner"></p>
      </section>

      <section class="result-section proverb-section">
        <h4>来自TA的箴言</h4>
        <p>“{{ result.proverb }}”</p>
      </section>
      
      <section class="result-section">
        <h2 class="section-title"><span class="icon">📊</span> 你的六维人格图谱</h2>
        <div class="chart-container">
          <canvas ref="chartRef"></canvas>
        </div>
      </section>
      
      <section class="result-section">
          <h2 class="section-title"><span class="icon">🎨</span> 你的恋爱风格倾向</h2>
          <div class="bar-chart-container">
              <canvas ref="barChartRef"></canvas>
          </div>
      </section>

      <button @click="retest" class="retest-button">🚀 再测一次</button>
       <footer class="footer">
        <p class="brand">小红书 ♥ 元认知星图</p>
      </footer>
    </div>
  </div>
</template>

<style scoped>
:root {
  --theme-primary: #ff6b81;
  --theme-secondary: #fde7f0;
  --theme-text: #d63031;
}
.result-container {
  padding: 20px; min-height: 100vh;
  background: linear-gradient(160deg, var(--theme-secondary), #f4f6ff);
  display: flex; justify-content: center; align-items: flex-start;
  transition: background 0.5s ease;
}
.card {
  width: 100%; max-width: 550px; background-color: white;
  border-radius: 28px; padding: 3rem;
  box-shadow: 0 20px 50px rgba(0, 0, 0, 0.12);
  margin: 2rem 0; animation: fadeIn 0.8s ease-out;
}
@keyframes fadeIn {
  from { opacity: 0; transform: translateY(30px); }
  to { opacity: 1; transform: translateY(0); }
}
.card-header { text-align: center; margin-bottom: 2.5rem; }
.animal-image {
  width: 150px; 
  height: 150px;
  border-radius: 50%;
  margin: 0 auto 1.5rem;
  border: 6px solid var(--theme-primary);
  box-shadow: 0 12px 35px color-mix(in srgb, var(--theme-primary) 40%, transparent);
  object-fit: cover;
}
.result-type { font-size: 1.3rem; color: #8c82a3; margin-bottom: 0.5rem; }
.animal-name {
  font-size: 3.5rem; font-weight: 700; color: var(--theme-text); line-height: 1.2;
}
.animal-title { font-size: 1.4rem; color: #6a6882; margin-top: 0.8rem; }
.result-section { margin-bottom: 3rem; }
.section-title {
  display: flex; align-items: center; font-size: 1.6rem;
  font-weight: 600; color: #3D3B56; margin-bottom: 1.5rem;
  padding-bottom: 1rem; border-bottom: 1px solid var(--theme-secondary);
}
.section-title .icon { font-size: 2rem; margin-right: 1rem; }
.description { font-size: 1.2rem; line-height: 1.9; color: #4A4A6A; }
.sub-description {
  margin-top: 1.5rem; padding: 1.5rem;
  background-color: color-mix(in srgb, var(--theme-secondary) 80%, white);
  border-radius: 16px; color: var(--theme-text); font-weight: 600;
}
.chemistry-content { padding-top: 0.5rem; }
.chemistry-title {
  font-size: 1.3rem; font-weight: 600; color: var(--theme-text); margin-bottom: 0.8rem;
}
.result-section p { line-height: 1.9; color: #4A4A6A; font-size: 1.2rem; }
.strengths-challenges-grid { display: grid; gap: 3rem; }
.custom-list { list-style: none; padding-left: 0; }
.custom-list li {
  position: relative; padding-left: 2rem; margin-bottom: 1rem;
  line-height: 1.8; color: #4A4A6A; font-size: 1.15rem;
}
.custom-list li::before {
  content: '✓'; position: absolute; left: 0;
  color: var(--theme-text); font-weight: bold; font-size: 1.3rem;
}
.proverb-section {
  background-color: var(--theme-secondary); border-left: 5px solid var(--theme-primary);
  border-radius: 16px; padding: 2rem; text-align: center;
}
.proverb-section h4 {
  font-size: 1.2rem; font-weight: 600; color: var(--theme-text);
  opacity: 0.8; margin-bottom: 0.8rem;
}
.proverb-section p {
  font-size: 1.4rem; font-style: italic; font-weight: 500;
  color: var(--theme-text); line-height: 1.8;
}
.chart-container { height: 400px; position: relative; }
.bar-chart-container { height: 350px; position: relative; margin-top: 1rem; }
.retest-button {
  width: 100%; padding: 1.2rem; font-size: 1.25rem;
  font-weight: bold; color: white; background-color: var(--theme-primary);
  border: none; border-radius: 18px; cursor: pointer; transition: all 0.3s ease;
  margin-top: 1.5rem;
}
.retest-button:hover {
  transform: translateY(-6px);
  box-shadow: 0 10px 30px color-mix(in srgb, var(--theme-primary) 40%, transparent);
}
.footer { text-align: center; margin-top: 2.5rem; }
.footer .brand { font-weight: 600; color: #8c82a3; font-size: 1rem; }
</style>

