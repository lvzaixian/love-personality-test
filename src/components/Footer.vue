<script setup>
// Footer.vue - 页脚组件（在不重构的前提下改进布局与装饰）
import mpsBadge from '../img/备案图标.png'
</script>

<template>
  <footer class="footer">
    <!-- 背景渐变融合效果（轻微叠加，保持与原有主题融合） -->
    <div class="bg-overlay" aria-hidden="true"></div>
    <!-- 发光分隔线 -->
    <div class="glow-divider" aria-hidden="true"></div>
    <div class="container">
      <div class="footer-content">
        <!-- 第1行：隐私与信任声明 -->
        <div class="privacy-statement">
          <span class="emoji">🔒</span>
          <span>您的数据仅存本地, 不会上传</span>
        </div>
        
        <!-- 第2行：版权声明 -->
        <p class="copyright">
          © 2025 元认知星图团队 版权所有
        </p>
        
        <!-- 第2.5行：举报邮箱（位于版权与备案之间，增强层次与可读性） -->
        <p class="report-email">
          <a href="mailto:jubao@meta-mind.cn" class="email-link">举报邮箱: jubao@meta-mind.cn</a>
        </p>

        <!-- 第3行：合规与联系方式（公安备案 + ICP备案） -->
        <div class="compliance-contact">
          <span class="inline-items">
            <img :src="mpsBadge" alt="公安备案图标" width="12" height="12" loading="lazy" class="mps-badge" />
            <a 
              href="https://beian.mps.gov.cn/#/query/webSearch?code=36110002000184" 
              target="_blank" 
              rel="noreferrer" 
              class="mps-link"
            >
              赣公网安备36110002000184号
            </a>
          </span>
          <span class="dot-sep">·</span>
          <a 
            href="https://beian.miit.gov.cn/" 
            target="_blank" 
            rel="noopener noreferrer" 
            class="beian"
          >
            赣ICP备2025075068号
          </a>
        </div>
      </div>
    </div>
    <!-- 装饰性微光效果（保留轻微，不影响信息可读性） -->
    <div class="glow-left" aria-hidden="true"></div>
    <div class="glow-right" aria-hidden="true"></div>
  </footer>
</template>

<style scoped>
.footer {
  width: 100%;
  padding: 20px 0;
  background: linear-gradient(135deg, var(--color-bg-start, #fde7f0), var(--color-bg-end, #e6e9ff));
  border-top: 1px solid rgba(255, 255, 255, 0.1);
  text-align: center;
  font-size: 1.1rem;
  color: var(--color-text-light);
  box-shadow: 0 -5px 15px rgba(0, 0, 0, 0.03);
  position: relative;
  overflow: hidden;
  backdrop-filter: blur(8px);
}

.container {
  width: 100%;
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 1rem;
}

.footer-content {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 0.75rem;
  text-align: center;
  font-size: 1.1rem;
  color: var(--color-text-light, #6a6882);
}

.privacy-statement {
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 1.1rem;
}

.emoji {
  margin-right: 0.5rem;
}

.copyright {
  margin: 0;
}

.report-email {
  margin: 0.25rem 0 0.25rem;
}

.email-link {
  color: var(--color-text-light);
  text-decoration: none;
}

.email-link:hover {
  color: var(--color-primary);
}

.compliance-contact {
  display: flex;
  flex-wrap: wrap;
  align-items: center;
  justify-content: center;
  gap: 0.5rem;
  font-size: 1.1rem;
  opacity: 0.8;
}

.inline-items {
  display: inline-flex;
  align-items: center;
  gap: 0.25rem;
  white-space: nowrap;
}

.mps-badge {
  opacity: 0.7;
}

.mps-link {
  color: inherit;
  text-decoration: none;
  transition: color 0.2s ease;
}

.mps-link:hover {
  color: var(--color-primary);
}

.beian {
  color: inherit;
  text-decoration: none;
  transition: color 0.2s ease;
}

.beian:hover {
  color: var(--color-primary, #ff6b81);
}

.divider {
  user-select: none;
  margin: 0 8px;
}

.dot-sep {
  margin: 0 6px;
  opacity: 0.5;
}

.bg-overlay {
  position: absolute;
  inset: 0;
  background: linear-gradient(90deg, rgba(76, 106, 255, 0.05), rgba(180, 76, 255, 0.05), rgba(255, 107, 129, 0.05));
  pointer-events: none;
}

.glow-divider {
  width: 8rem;
  height: 1px;
  margin: 0 auto 0.75rem;
  background: linear-gradient(90deg, transparent, rgba(99, 102, 241, 0.5), transparent);
}

.glow-left,
.glow-right {
  position: absolute;
  bottom: 0;
  width: 10rem;
  height: 10rem;
  border-radius: 50%;
  filter: blur(80px);
}

.glow-left {
  left: 25%;
  background: rgba(99, 102, 241, 0.1);
}

.glow-right {
  right: 25%;
  background: rgba(236, 72, 153, 0.1);
}

/* 移动端适配 */
@media (max-width: 480px) {
  .footer-content {
    font-size: 1.1rem;
  }
  
  .report-email {
    font-size: 1rem;
  }

  .compliance-contact {
    flex-direction: column;
    gap: 0.25rem;
  }
  
  .divider {
    display: none;
  }
}
</style>