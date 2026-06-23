<script setup>
import Navbar from './components/Navbar.vue';
import Hero from './components/Hero.vue';
import About from './components/About.vue';
import Projects from './components/Projects.vue';
import Contact from './components/Contact.vue';
import { onMounted } from 'vue';

// Scroll-triggered fade-up
onMounted(() => {
  const obs = new IntersectionObserver((entries) => {
    entries.forEach(e => {
      if (e.isIntersecting) {
        e.target.classList.add('visible');
        obs.unobserve(e.target);
      }
    });
  }, { threshold: 0.15 });

  document.querySelectorAll('.fade-up:not(.visible)').forEach(el => obs.observe(el));
});

const scrollTop = () => window.scrollTo({ top: 0, behavior: 'smooth' });
</script>

<template>
  <div class="app-wrapper">
    <!-- Background glows -->
    <div class="bg-glow bg-glow-1"></div>
    <div class="bg-glow bg-glow-2"></div>

    <Navbar />
    <main>
      <Hero />
      <About />
      <Projects />
      <Contact />
    </main>

    <footer class="footer">
      <div class="container footer-inner">
        <a href="#hero" class="footer-logo">
          <span class="text-gradient">&lt;Portfolio /&gt;</span>
        </a>
        <p class="footer-copy">
          © 2026 Ngô Văn Gia Huy — Thiết kế với
          <i class="fa-solid fa-heart" style="color:#f472b6; margin: 0 3px;"></i>
          &amp; nhiều cà phê ☕
        </p>
        <button class="back-to-top" @click="scrollTop" aria-label="Lên đầu trang" id="back-to-top">
          <i class="fa-solid fa-arrow-up"></i>
        </button>
      </div>
      <div class="footer-gradient-bar"></div>
    </footer>
  </div>
</template>

<style>
/* Global app wrapper */
.app-wrapper {
  min-height: 100vh;
  position: relative;
}
</style>

<style scoped>
.footer {
  position: relative;
  padding: 2.5rem 0 0;
  overflow: hidden;
}

.footer-inner {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding-bottom: 2rem;
  border-top: 1px solid var(--border-glass);
  padding-top: 1.75rem;
  gap: 1rem;
  flex-wrap: wrap;
}

.footer-logo {
  font-size: 1.2rem;
  font-weight: 800;
  text-decoration: none;
  font-family: var(--font-code);
}

.footer-copy {
  color: var(--text-muted);
  font-size: 0.9rem;
  text-align: center;
}

/* ── Back to top ── */
.back-to-top {
  width: 42px;
  height: 42px;
  border-radius: 50%;
  background: var(--bg-glass);
  border: 1px solid var(--border-glass);
  color: var(--text-muted);
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 1rem;
  transition: var(--transition);
  flex-shrink: 0;
}

.back-to-top:hover {
  background: var(--accent-gradient);
  color: #fff;
  border-color: transparent;
  transform: translateY(-3px);
  box-shadow: 0 8px 20px var(--shadow-glow);
}

/* ── Bottom gradient bar ── */
.footer-gradient-bar {
  height: 3px;
  background: var(--aurora-gradient);
  background-size: 300% 100%;
  animation: aurora 5s linear infinite;
  opacity: 0.7;
}

@media (max-width: 600px) {
  .footer-inner { flex-direction: column; text-align: center; }
  .back-to-top  { margin: 0 auto; }
}
</style>
