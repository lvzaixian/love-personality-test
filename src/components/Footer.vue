<script setup>
// Footer.vue - 页脚组件
</script>

<template>
  <footer class="footer">
    <div class="container">
      <div class="footer-content">
        <!-- 第1行：隐私与信任声明 -->
        <div class="privacy-statement">
          <span class="emoji">🔒</span>
          <span>您的数据仅存本地, 不会上传</span>
        </div>
        
        <!-- 第2行：版权声明 -->
        <p class="copyright">
          © 2025 元认知星图 版权所有
        </p>
        
        <!-- 第3行：合规与联系方式 -->
        <div class="footer-links">
          <a href="/privacy" class="footer-link">隐私声明</a>
          <span class="separator">|</span>
          <a href="https://beian.miit.gov.cn/" target="_blank" class="footer-link">赣ICP备2025075068号</a>
          <span class="separator">|</span>
          <a href="mailto:jubao@meta-mind.cn" class="footer-link">联系我们</a>
        </div>
      </div>
    </div>
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
  font-size: 0.875rem;
  color: var(--color-text-light, #6a6882);
}

.privacy-statement {
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 0.9375rem;
}

.emoji {
  margin-right: 0.5rem;
}

.copyright {
  margin: 0;
}

.footer-links {
  margin-top: 15px;
}

.footer-link {
  color: var(--color-text-light);
  text-decoration: none;
  font-size: 1.1rem;
  transition: color 0.2s;
}

.footer-link:hover {
  color: var(--color-primary);
}

.separator {
  margin: 0 10px;
  color: var(--color-text-light);
  opacity: 0.6;
}

/* 移动端适配 */
@media (max-width: 480px) {
  .footer {
    padding: 1.25rem 0;
  }
  
  .footer-content {
    gap: 0.5rem;
    font-size: 0.8125rem;
  }
  
  .privacy-statement {
    font-size: 0.875rem;
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