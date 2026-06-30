<script setup>
import Navbar from './components/Navbar.vue';
import Hero from './components/Hero.vue';
import About from './components/About.vue';
import Projects from './components/Projects.vue';
import Contact from './components/Contact.vue';
import LoadingScreen from './components/LoadingScreen.vue';
import { onMounted, onUnmounted, ref, watch, nextTick } from 'vue';
import Lenis from 'lenis';
import gsap from 'gsap';

if ('scrollRestoration' in history) {
  history.scrollRestoration = 'manual';
}
window.scrollTo(0, 0);

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

const mouseX = ref(window.innerWidth / 2);
const mouseY = ref(window.innerHeight / 2);
const isHovering = ref(false);
const isLoading = ref(true);

const handleLoadingComplete = () => {
  gsap.set('.greeting, .name', { opacity: 0, y: 80, filter: 'blur(5px)' });
  gsap.set('.hero-right', { opacity: 0, y: 30 });
  gsap.set(['nav', '.social-sidebar'], { opacity: 0 });

  isLoading.value = false;

  nextTick(() => {
    gsap.to('.greeting', { opacity: 1, y: 0, filter: 'blur(0px)', duration: 1.2, ease: 'power3.inOut', delay: 0.3 });
    gsap.to('.name', { opacity: 1, y: 0, filter: 'blur(0px)', duration: 1.2, ease: 'power3.inOut', delay: 0.4 });
    gsap.to('.hero-right', { opacity: 1, y: 0, duration: 1.2, ease: 'power1.inOut', delay: 0.8 });
    gsap.to(['nav', '.social-sidebar'], { opacity: 1, duration: 1.2, ease: 'power1.inOut', delay: 0.1 });
  });
};

watch(isLoading, (newVal) => {
  if (newVal) {
    document.body.style.overflow = 'hidden';
  } else {
    document.body.style.overflow = '';
  }
}, { immediate: true });

const handleMouseMove = (e) => {
  mouseX.value = e.clientX;
  mouseY.value = e.clientY;
};

const handleMouseOver = (e) => {
  if (e.target.closest('a, button, [role="button"], .filter-btn, .slide-btn, .social-sidebar')) {
    isHovering.value = true;
  } else {
    isHovering.value = false;
  }
};

onMounted(() => {
  const lenis = new Lenis({
    duration: 1.2,
    easing: (t) => Math.min(1, 1.001 - Math.pow(2, -10 * t)),
    smooth: true,
  });

  function raf(time) {
    lenis.raf(time);
    requestAnimationFrame(raf);
  }
  requestAnimationFrame(raf);

  const ANIM_CLASSES = '.fade-up, .fade-left, .fade-right, .fade-scale, .fade-blur';
  const fadeObs = new IntersectionObserver((entries) => {
    entries.forEach(e => {
      if (e.isIntersecting) { e.target.classList.add('visible'); fadeObs.unobserve(e.target); }
    });
  }, { threshold: 0.1, rootMargin: '0px 0px -60px 0px' });
  document.querySelectorAll(`${ANIM_CLASSES}:not(.visible)`).forEach(el => fadeObs.observe(el));

  const FLIP_CLASSES = '.flip-in-bottom, .flip-in-top, .flip-in-right, .flip-in-left';
  const flipObs = new IntersectionObserver((entries) => {
    entries.forEach(e => {
      if (e.isIntersecting) { e.target.classList.add('flip-active'); flipObs.unobserve(e.target); }
    });
  }, { threshold: 0.12, rootMargin: '0px 0px -40px 0px' });
  document.querySelectorAll(FLIP_CLASSES).forEach(el => flipObs.observe(el));

  splitAndAnimate('.split-text');
  const splitObs = new IntersectionObserver((entries) => {
    entries.forEach(e => {
      if (e.isIntersecting) { e.target.classList.add('split-visible'); splitObs.unobserve(e.target); }
    });
  }, { threshold: 0.25 });
  document.querySelectorAll('.split-text').forEach(el => splitObs.observe(el));

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

  window.addEventListener('mousemove', handleMouseMove, { passive: true });
  window.addEventListener('mouseover', handleMouseOver, { passive: true });
});

onUnmounted(() => {
  window.removeEventListener('mousemove', handleMouseMove);
  window.removeEventListener('mouseover', handleMouseOver);
});

const scrollTop = () => window.scrollTo({ top: 0, behavior: 'smooth' });
</script>

<template>
  <div class="app-wrapper">
    <LoadingScreen v-if="isLoading" @complete="handleLoadingComplete" />

    <div class="cursor-glow"
      :style="{ transform: `translate(${mouseX}px, ${mouseY}px) translate(-50%, -50%) ${isHovering ? 'scale(0)' : 'scale(1)'}` }">
    </div>

    <div class="bg-glow bg-glow-1"></div>
    <div class="bg-glow bg-glow-2"></div>

    <Navbar />
    <main>
      <Hero />


      <About />



      <Projects />
      <Contact />
    </main>

  </div>
</template>

<style>
.app-wrapper {
  min-height: 100vh;
  position: relative;
}

.cursor-glow {
  position: fixed;
  top: 0;
  left: 0;
  width: 50px;
  height: 50px;
  border-radius: 50%;
  background: rgb(226, 198, 255);
  box-shadow: 0 0 20px 2px rgb(214, 173, 255);
  pointer-events: none;
  mix-blend-mode: difference;
  z-index: 9999;
  will-change: transform;
  transition: transform 0.15s ease-out;
}

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

.marquee-section {
  width: 100%;
  overflow: hidden;
  padding: 1.25rem 0;
  border-top: 1px solid var(--border-glass);
  border-bottom: 1px solid var(--border-glass);
  background: rgba(255, 255, 255, 0.02);
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
  color: rgba(255, 255, 255, 0.2);
}

.mdot {
  color: var(--accent);
  opacity: 0.7;
  font-size: 0.7em;
}

.marquee-reverse .mdot {
  color: var(--accent-alt, #a78bfa);
}

@keyframes marquee-fwd {
  from {
    transform: translateX(0);
  }

  to {
    transform: translateX(-50%);
  }
}

@keyframes marquee-rev {
  from {
    transform: translateX(-50%);
  }

  to {
    transform: translateX(0);
  }
}

.marquee-section:hover .marquee-track {
  animation-play-state: paused;
}

@media (prefers-reduced-motion: reduce) {
  .marquee-track {
    animation: none;
  }

  .split-char {
    transform: none;
    opacity: 1;
    transition: none;
  }
}
</style>
