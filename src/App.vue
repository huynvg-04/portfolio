<script setup>
import Navbar from './components/Navbar.vue';
import Hero from './components/Hero.vue';
import About from './components/About.vue';
import Projects from './components/Projects.vue';
import Contact from './components/Contact.vue';
import { onMounted } from 'vue';

// ── Split Text helper ──
const splitAndAnimate = (selector) => {
  document.querySelectorAll(selector).forEach(el => {
    if (el.dataset.split) return;
    el.dataset.split = 'true';
    const words = el.textContent.split(' ');
    el.textContent = '';
    words.forEach((word, wi) => {
      const wordWrap = document.createElement('span');
      wordWrap.style.cssText = 'display:inline-block;overflow:hidden;vertical-align:bottom;';
      word.split('').forEach((char, ci) => {
        const span = document.createElement('span');
        span.textContent = char;
        span.className = 'split-char';
        span.style.setProperty('--i', wi * 8 + ci);
        wordWrap.appendChild(span);
      });
      el.appendChild(wordWrap);
      if (wi < words.length - 1) el.appendChild(document.createTextNode('\u00a0'));
    });
  });
};

let scrollTicking = false;

onMounted(() => {
  // ── Fade animations ──
  const ANIM_CLASSES = '.fade-up, .fade-left, .fade-right, .fade-scale, .fade-blur';
  const fadeObs = new IntersectionObserver((entries) => {
    entries.forEach(e => {
      if (e.isIntersecting) { e.target.classList.add('visible'); fadeObs.unobserve(e.target); }
    });
  }, { threshold: 0.1, rootMargin: '0px 0px -60px 0px' });
  document.querySelectorAll(`${ANIM_CLASSES}:not(.visible)`).forEach(el => fadeObs.observe(el));

  // ── 3D Flip animations ──
  const FLIP_CLASSES = '.flip-in-bottom, .flip-in-top, .flip-in-right, .flip-in-left';
  const flipObs = new IntersectionObserver((entries) => {
    entries.forEach(e => {
      if (e.isIntersecting) { e.target.classList.add('flip-active'); flipObs.unobserve(e.target); }
    });
  }, { threshold: 0.12, rootMargin: '0px 0px -40px 0px' });
  document.querySelectorAll(FLIP_CLASSES).forEach(el => flipObs.observe(el));

  // ── Split Text Reveal ──
  splitAndAnimate('.split-text');
  const splitObs = new IntersectionObserver((entries) => {
    entries.forEach(e => {
      if (e.isIntersecting) { e.target.classList.add('split-visible'); splitObs.unobserve(e.target); }
    });
  }, { threshold: 0.25 });
  document.querySelectorAll('.split-text').forEach(el => splitObs.observe(el));

  // ── Parallax ──
  window.addEventListener('scroll', () => {
    if (scrollTicking) return;
    scrollTicking = true;
    requestAnimationFrame(() => {
      const y = window.scrollY;
      document.querySelectorAll('[data-parallax]').forEach(el => {
        el.style.transform = `translateY(${y * parseFloat(el.dataset.parallax || 0.3)}px)`;
      });
      scrollTicking = false;
    });
  }, { passive: true });
});

const scrollTop = () => window.scrollTo({ top: 0, behavior: 'smooth' });
</script>

<template>
  <div class="app-wrapper">
    <div class="bg-glow bg-glow-1"></div>
    <div class="bg-glow bg-glow-2"></div>

    <Navbar />
    <main>
      <Hero />

      <!-- Marquee 1: Roles -->
      <div class="marquee-section" aria-hidden="true">
        <div class="marquee-track">
          <div class="marquee-content">
            <span>Full-Stack Developer</span><span class="mdot">✦</span>
            <span>Vue.js Specialist</span><span class="mdot">✦</span>
            <span>UI/UX Enthusiast</span><span class="mdot">✦</span>
            <span>Problem Solver</span><span class="mdot">✦</span>
            <span>Creative Coder</span><span class="mdot">✦</span>
            <span>Open to Work</span><span class="mdot">✦</span>
          </div>
          <div class="marquee-content" aria-hidden="true">
            <span>Full-Stack Developer</span><span class="mdot">✦</span>
            <span>Vue.js Specialist</span><span class="mdot">✦</span>
            <span>UI/UX Enthusiast</span><span class="mdot">✦</span>
            <span>Problem Solver</span><span class="mdot">✦</span>
            <span>Creative Coder</span><span class="mdot">✦</span>
            <span>Open to Work</span><span class="mdot">✦</span>
          </div>
        </div>
      </div>

      <About />

      <!-- Marquee 2: Tech (reverse) -->
      <div class="marquee-section marquee-reverse" aria-hidden="true">
        <div class="marquee-track">
          <div class="marquee-content">
            <span>PHP</span><span class="mdot">◆</span>
            <span>Laravel</span><span class="mdot">◆</span>
            <span>Vue.js</span><span class="mdot">◆</span>
            <span>Node.js</span><span class="mdot">◆</span>
            <span>MySQL</span><span class="mdot">◆</span>
            <span>Vite</span><span class="mdot">◆</span>
            <span>Git</span><span class="mdot">◆</span>
            <span>Figma</span><span class="mdot">◆</span>
          </div>
          <div class="marquee-content" aria-hidden="true">
            <span>PHP</span><span class="mdot">◆</span>
            <span>Laravel</span><span class="mdot">◆</span>
            <span>Vue.js</span><span class="mdot">◆</span>
            <span>Node.js</span><span class="mdot">◆</span>
            <span>MySQL</span><span class="mdot">◆</span>
            <span>Vite</span><span class="mdot">◆</span>
            <span>Git</span><span class="mdot">◆</span>
            <span>Figma</span><span class="mdot">◆</span>
          </div>
        </div>
      </div>

      <Projects />
      <Contact />
    </main>

    <footer class="footer">
      <div class="container footer-inner">
        <a href="#hero" class="footer-logo">
          <span class="text-gradient">&lt;huynvg-04/&gt;</span>
        </a>
        <p class="footer-copy">
          &copy; 2026 Ng&ocirc; V&#259;n Gia Huy &mdash; Thi&#7871;t k&#7871; v&#7899;i
          <i class="fa-solid fa-heart" style="color:#f472b6; margin: 0 3px;"></i>
          &amp; nhi&#7873;u c&agrave; ph&ecirc; &#9749;
        </p>
        <button class="back-to-top" @click="scrollTop" aria-label="L&#234;n &#273;&#7847;u trang" id="back-to-top">
          <i class="fa-solid fa-arrow-up"></i>
        </button>
      </div>
      <div class="footer-gradient-bar"></div>
    </footer>
  </div>
</template>

<style>
.app-wrapper { min-height: 100vh; position: relative; }

/* ── Split Text Animation ── */
.split-char {
  display: inline-block;
  transform: translateY(100%);
  opacity: 0;
  transition:
    transform 0.65s cubic-bezier(0.23, 1, 0.32, 1),
    opacity 0.45s ease;
  transition-delay: calc(var(--i, 0) * 0.03s);
}
.split-visible .split-char {
  transform: translateY(0);
  opacity: 1;
}

/* ── Marquee ── */
.marquee-section {
  width: 100%;
  overflow: hidden;
  padding: 1.25rem 0;
  border-top: 1px solid var(--border-glass);
  border-bottom: 1px solid var(--border-glass);
  background: rgba(255,255,255,0.02);
  position: relative;
  z-index: 2;
}
.marquee-track {
  display: flex;
  width: max-content;
  animation: marquee-fwd 30s linear infinite;
}
.marquee-reverse .marquee-track {
  animation: marquee-rev 24s linear infinite;
}
.marquee-content {
  display: flex;
  align-items: center;
  gap: 2.5rem;
  padding: 0 2.5rem;
  white-space: nowrap;
  font-size: clamp(0.95rem, 1.8vw, 1.25rem);
  font-weight: 600;
  color: var(--text-muted);
  letter-spacing: 0.01em;
  min-width: 100vw;
}
.marquee-reverse .marquee-content {
  font-family: var(--font-code);
  font-size: clamp(1rem, 2vw, 1.4rem);
  font-weight: 700;
  letter-spacing: 0.08em;
  text-transform: uppercase;
  color: rgba(255,255,255,0.2);
}
.mdot { color: var(--accent); opacity: 0.7; font-size: 0.7em; }
.marquee-reverse .mdot { color: var(--accent-alt, #a78bfa); }

@keyframes marquee-fwd {
  from { transform: translateX(0); }
  to   { transform: translateX(-50%); }
}
@keyframes marquee-rev {
  from { transform: translateX(-50%); }
  to   { transform: translateX(0); }
}
.marquee-section:hover .marquee-track { animation-play-state: paused; }

@media (prefers-reduced-motion: reduce) {
  .marquee-track { animation: none; }
  .split-char { transform: none; opacity: 1; transition: none; }
}
</style>

<style scoped>
.footer { position: relative; padding: 2.5rem 0 0; overflow: hidden; }
.footer-inner {
  display: flex; align-items: center; justify-content: space-between;
  padding-bottom: 2rem; border-top: 1px solid var(--border-glass);
  padding-top: 1.75rem; gap: 1rem; flex-wrap: wrap;
}
.footer-logo { font-size: 1.2rem; font-weight: 800; text-decoration: none; font-family: var(--font-code); }
.footer-copy { color: var(--text-muted); font-size: 0.9rem; text-align: center; }
.back-to-top {
  width: 42px; height: 42px; border-radius: 50%;
  background: var(--bg-glass); border: 1px solid var(--border-glass);
  color: var(--text-muted); cursor: pointer; display: flex;
  align-items: center; justify-content: center; font-size: 1rem;
  transition: var(--transition); flex-shrink: 0;
}
.back-to-top:hover {
  background: var(--accent-gradient); color: #fff; border-color: transparent;
  transform: translateY(-3px); box-shadow: 0 8px 20px var(--shadow-glow);
}
.footer-gradient-bar {
  height: 3px; background: var(--aurora-gradient); background-size: 300% 100%;
  animation: aurora 5s linear infinite; opacity: 0.7;
}
@media (max-width: 600px) {
  .footer-inner { flex-direction: column; text-align: center; }
  .back-to-top  { margin: 0 auto; }
}
</style>
