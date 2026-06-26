<script setup>
</script>

<template>
  <section id="hero" class="hero-section">
    <div class="social-sidebar fade-right visible" data-delay="100">
      <a href="https://github.com/huynvg-04/" target="_blank" rel="noopener" aria-label="GitHub">
        <i class="fa-brands fa-github"></i>
      </a>
      <a href="https://www.linkedin.com/in/ngo-van-gia-huy04" target="_blank" rel="noopener" aria-label="LinkedIn">
        <i class="fa-brands fa-linkedin-in"></i>
      </a>
      <a href="https://www.facebook.com/em.an.com.chua.UwU" target="_blank" rel="noopener" aria-label="Facebook">
        <i class="fa-brands fa-facebook-f"></i>
      </a>
    </div>

    <div class="container hero-content">
      
      <div class="hero-left fade-up visible" data-delay="200">
        <span class="greeting">Hello! I'm</span>
        <h1 class="name">
          <span>NGÔ VĂN</span>
          <span>GIA HUY</span>
        </h1>
      </div>

      <div class="hero-right fade-left visible" data-delay="300">
        <span class="role-prefix">A Creative</span>
        <div class="role-group">
          <span class="role-bg">DEVELOPER</span>
          <span class="role-fg">FULL-STACK</span>
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
  position: relative;
  overflow: hidden;
  padding-top: 60px;
}

.social-sidebar {
  position: absolute;
  left: 3rem;
  top: 50%;
  transform: translateY(-50%);
  display: flex;
  flex-direction: column;
  gap: 2rem;
  z-index: 10;
}

.social-sidebar a {
  color: var(--text-muted);
  font-size: 1.4rem;
  text-decoration: none;
  transition: var(--transition);
  display: flex;
  align-items: center;
  justify-content: center;
}

.social-sidebar a:hover {
  color: #fff;
  transform: scale(1.15) translateY(-2px);
  text-shadow: 0 0 10px rgba(214, 173, 255, 0.4);
}

.hero-content {
  display: flex;
  justify-content: space-between;
  align-items: center;
  width: 100%;
  max-width: 1400px;
  margin: 0 auto;
  padding: 0 6rem 0 9rem; /* Left padding ensures it clears the sidebar */
  position: relative;
  z-index: 1;
}

.greeting, .role-prefix {
  display: block;
  font-family: 'Space Grotesk', sans-serif;
  font-size: clamp(1.1rem, 1.8vw, 1.5rem);
  color: var(--accent);
  margin-bottom: 0.5rem;
  font-weight: 400;
  letter-spacing: 0.04em;
  font-style: italic;
}

.hero-left {
  display: flex;
  flex-direction: column;
}

.name {
  display: flex;
  flex-direction: column;
  line-height: 1.05;
}

.name span {
  font-family: 'Space Grotesk', sans-serif;
  font-size: clamp(1.5rem, 2vw, 6rem);
  font-weight: 700;
  color: #fff;
  letter-spacing: -0.02em;
  text-transform: uppercase;
}

.hero-right {
  display: flex;
  flex-direction: column;
  align-items: flex-end;
  text-align: right;
}

.role-group {
  position: relative;
  display: inline-block;
  margin-top: -0.5rem;
}

.role-bg {
  display: block;
  font-family: 'Bebas Neue', sans-serif;
  font-size: clamp(5rem, 10vw, 9rem);
  font-weight: 400;
  color: transparent;
  -webkit-text-stroke: 2px rgba(214, 173, 255, 0.5);
  letter-spacing: 0.06em;
  line-height: 1;
  text-transform: uppercase;
}

.role-fg {
  position: absolute;
  top: 55%;
  right: 0;
  transform: translateY(-50%);
  font-family: 'Bebas Neue', sans-serif;
  font-size: clamp(3.5rem, 7vw, 6.5rem);
  font-weight: 400;
  color: #fff;
  letter-spacing: 0.04em;
  white-space: nowrap;
  text-shadow: 0 4px 30px rgba(0,0,0,0.9);
}

@media (max-width: 1024px) {
  .hero-content {
    flex-direction: column;
    justify-content: center;
    gap: 5rem;
    padding: 0 3rem 0 6rem;
    text-align: center;
  }
  
  .hero-left {
    align-items: center;
  }
  
  .hero-right {
    align-items: center;
    text-align: center;
  }
  
  .role-fg {
    right: 50%;
    transform: translate(50%, -50%);
  }
}

@media (max-width: 768px) {
  .hero-content {
    padding: 0 1.5rem;
  }
  
  .social-sidebar {
    display: none;
  }
  
  .name span { font-size: 3rem; }
  .role-bg { font-size: 4.5rem; }
  .role-fg { font-size: 3rem; }
}

@media (max-width: 480px) {
  .name span { font-size: 2.4rem; }
  .role-bg { font-size: 3.5rem; -webkit-text-stroke: 1.5px rgba(214, 173, 255, 0.5); }
  .role-fg { font-size: 2.5rem; }
  .greeting, .role-prefix { font-size: 1rem; }
}
</style>
