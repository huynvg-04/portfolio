<script setup>
// Hero Section
</script>

<template>
  <section id="hero" class="hero-section">
    <div class="bg-glow top-right"></div>
    <div class="container hero-content">
      <div class="hero-text">
        <span class="badge">Sẵn sàng nhận việc</span>
        <h1 class="hero-title">
          Xin chào, tôi là <br />
          <span class="text-gradient">Lập trình viên Full-Stack</span> <span class="wave-emoji">👋</span>
        </h1>
        <p class="hero-subtitle">
          Tôi xây dựng các trải nghiệm web cao cấp, nhanh chóng và dễ tiếp cận, kết hợp hoàn hảo giữa thiết kế tinh tế và công nghệ hiện đại.
        </p>
        <div class="hero-actions">
          <a href="#projects" class="btn btn-primary">
            Xem Sản Phẩm <i class="fa-solid fa-arrow-right"></i>
          </a>
          <div class="social-links">
            <a href="https://github.com/cornhuy04" class="social-icon"><i class="fa-brands fa-github"></i></a>
            <a href="https://www.linkedin.com/in/ngo-van-gia-huy04" class="social-icon"><i class="fa-brands fa-linkedin"></i></a>
            <a href="https://www.facebook.com/em.an.com.chua.UwU" class="social-icon"><i class="fa-brands fa-facebook"></i></a>
          </div>
        </div>
      </div>
      <div class="hero-visual animate-float">
        <div class="glass-card visual-card">
          <div class="code-block">
            <span class="keyword">const</span> <span class="variable">developer</span> = {<br>
            &nbsp;&nbsp;skills: [<span class="string">'Vue.js'</span>, <span class="string">'Vite'</span>, <span class="string">'CSS'</span>],<br>
            &nbsp;&nbsp;passion: <span class="string">'Giao diện tinh tế'</span><br>
            };
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<style scoped>
.hero-section {
  min-height: 100vh;
  display: flex;
  align-items: center;
  padding-top: 80px;
}

.hero-content {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 4rem;
  align-items: center;
}

.badge {
  display: inline-block;
  padding: 0.5rem 1rem;
  background: var(--bg-glass);
  border: 1px solid var(--border-glass);
  border-radius: 20px;
  font-size: 0.875rem;
  font-weight: 500;
  margin-bottom: 2rem;
  color: var(--text-muted);
}

.hero-title {
  font-size: 4.5rem;
  margin-bottom: 1.5rem;
  letter-spacing: -0.02em;
}

.hero-subtitle {
  font-size: 1.25rem;
  color: var(--text-muted);
  max-width: 500px;
  margin-bottom: 2.5rem;
}

.hero-actions {
  display: flex;
  align-items: center;
  gap: 2rem;
}

.social-links {
  display: flex;
  gap: 1rem;
}

.social-icon {
  width: 45px;
  height: 45px;
  border-radius: 50%;
  background: var(--bg-glass);
  display: flex;
  align-items: center;
  justify-content: center;
  color: var(--text-main);
  text-decoration: none;
  font-size: 1.25rem;
  transition: var(--transition);
  border: 1px solid var(--border-glass);
}

.social-icon:hover {
  background: var(--accent);
  color: #fff;
  transform: translateY(-3px);
  border-color: var(--accent);
}

.visual-card {
  padding: 3rem;
  background: var(--bg-glass);
  border: 1px solid var(--border-glass);
  box-shadow: 0 25px 50px -12px var(--shadow-color);
}

.code-block {
  font-family: 'Fira Code', monospace;
  font-size: 1.1rem;
  line-height: 1.8;
  color: var(--text-main);
}

.keyword { color: #f472b6; }
.variable { color: #38bdf8; }
.string { color: #a3e635; }

:root[data-theme="light"] .keyword { color: #db2777; }
:root[data-theme="light"] .variable { color: #0284c7; }
:root[data-theme="light"] .string { color: #65a30d; }

@media (max-width: 992px) {
  .hero-content {
    grid-template-columns: 1fr;
    text-align: center;
  }
  .hero-subtitle { margin: 0 auto 2.5rem auto; }
  .hero-actions { justify-content: center; flex-direction: column; }
  .hero-title { font-size: 3.5rem; }
}
</style>
