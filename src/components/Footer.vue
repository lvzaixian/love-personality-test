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
          © 2025 元认知星图团队 版权所有
        </p>
        
        <!-- 第3行：合规与联系方式 -->
        <div class="compliance-contact">
          <a 
            href="https://beian.miit.gov.cn/" 
            target="_blank" 
            rel="noopener noreferrer" 
            class="beian"
          >
            赣ICP备2025075068号
          </a>
          
          <span class="divider">|</span>
          
          <span class="contact">
            举报邮箱: jubao@meta-mind.cn
          </span>
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

.compliance-contact {
  display: flex;
  flex-wrap: wrap;
  align-items: center;
  justify-content: center;
  gap: 0.5rem;
  font-size: 1.1rem;
  opacity: 0.8;
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

/* 移动端适配 */
@media (max-width: 480px) {
  .footer-content {
    font-size: 1.1rem;
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