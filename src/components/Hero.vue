<script setup>
import { ref, onMounted, onUnmounted } from 'vue';

const CHARS = 'ABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789@#$%&';
const topWord = 'FULL-STACK';
const bottomWord = 'DEVELOPER';

const topDisplay = ref(topWord.split('').map(() => ' '));
const bottomDisplay = ref(bottomWord.split('').map(() => ' '));

const lockedTop = ref(topWord.split('').map(() => false));
const lockedBottom = ref(bottomWord.split('').map(() => false));

let timers = [];

function randomChar() {
  return CHARS[Math.floor(Math.random() * CHARS.length)];
}

function slotReveal(targetWord, displayRef, lockedRef, startDelay) {
  targetWord.split('').forEach((finalChar, i) => {
    if (finalChar === '-') {
      setTimeout(() => {
        displayRef.value[i] = '‑';
        lockedRef.value[i] = true;
      }, startDelay + i * 180);
      return;
    }

    const spinDuration = 900 + i * 160;
    const interval = 60;
    const spinEnd = startDelay + i * 180 + spinDuration;
    let t = startDelay;

    const spin = () => {
      if (Date.now() >= spinEnd) {
        displayRef.value[i] = finalChar;
        lockedRef.value[i] = true;
        return;
      }
      if (!lockedRef.value[i]) displayRef.value[i] = randomChar();
      const id = setTimeout(spin, interval);
      timers.push(id);
    };

    const id = setTimeout(spin, startDelay + i * 120);
    timers.push(id);
  });
}

function runAnimation() {
  lockedTop.value = topWord.split('').map(() => false);
  lockedBottom.value = bottomWord.split('').map(() => false);
  topDisplay.value = topWord.split('').map(() => randomChar());
  bottomDisplay.value = bottomWord.split('').map(() => randomChar());

  slotReveal(topWord, topDisplay, lockedTop, 0);
  slotReveal(bottomWord, bottomDisplay, lockedBottom, 300);
}

let loopId = null;

onMounted(() => {
  runAnimation();
  loopId = setInterval(() => {
    timers.forEach(clearTimeout);
    timers = [];
    runAnimation();
  }, 5000);
});

onUnmounted(() => {
  timers.forEach(clearTimeout);
  if (loopId) clearInterval(loopId);
});
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
          <span>NGO VAN</span>
          <span>GIA HUY</span>
        </h1>
      </div>

      <div class="hero-right fade-left visible" data-delay="300">
        <span class="role-prefix">A Creative</span>
        <div class="kt-block">
          <!-- Top row: FULL-STACK — large neon purple -->
          <div class="kt-row kt-bg">
            <span v-for="(char, i) in topDisplay" :key="i" class="kt-char" :class="{ locked: lockedTop[i] }">{{ char
            }}</span>
          </div>
          <!-- Bottom row: DEVELOPER — solid white -->
          <div class="kt-row kt-fg">
            <span v-for="(char, i) in bottomDisplay" :key="i" class="kt-char" :class="{ locked: lockedBottom[i] }">{{
              char }}</span>
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
  padding: 0 6rem 0 9rem;
  position: relative;
  z-index: 1;
}

.greeting,
.role-prefix {
  display: block;
  font-family: 'Space Grotesk', sans-serif;
  font-size: clamp(1.1rem, 1.8vw, 1.5rem);
  color: var(--accent);
  margin-bottom: 0.5rem;
  font-weight: 400;
  letter-spacing: 0.04em;
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

/* ── Kinetic block ── */
.kt-block {
  display: flex;
  flex-direction: column;
  align-items: flex-end;
  line-height: 1;
  margin-top: 0.25rem;
  position: relative;
}

.kt-row {
  display: flex;
  align-items: baseline;
  justify-content: flex-end;
  position: relative;
}

.kt-fg {
  margin-top: -2em;
  z-index: 2;
}

.kt-bg {
  z-index: 1;
}

/* Individual characters */
.kt-char {
  display: inline-block;
  font-family: 'Space Grotesk', sans-serif;
  font-weight: 900;
  text-transform: uppercase;
  letter-spacing: -0.02em;
  transition: color 0.12s ease, text-shadow 0.2s ease;
  box-shadow: 50px #000000;
  text-shadow: 50px #000000;
}

/* Top row — FULL-STACK: large, neon purple outline */
.kt-bg .kt-char {
  font-size: clamp(2.8rem, 5.5vw, 5rem);
  color: rgba(180, 120, 255, 0.5);
  -webkit-text-stroke: 1.5px rgba(180, 120, 255, 0.75);
  filter: drop-shadow(0 0 10px rgba(160, 80, 255, 0.4));
}

.kt-bg .kt-char.locked {
  color: transparent;
  -webkit-text-stroke: 1.5px rgba(180, 120, 255, 0.75);
  filter: drop-shadow(0 0 14px rgba(160, 80, 255, 0.6));
}

/* Bottom row — DEVELOPER: solid white */
.kt-fg .kt-char {
  font-size: clamp(2rem, 4vw, 3.8rem);
  color: rgba(200, 200, 200, 0.45);
  text-shadow: none;
}

.kt-fg .kt-char.locked {
  color: #ffffff;
  text-shadow:
    0 4px 24px rgba(0, 0, 0, 0.8),
    0 0 40px rgba(200, 160, 255, 0.15);
  filter: drop-shadow(0 6px 18px rgba(0, 0, 0, 0.6));
}

/* ── Responsive ── */
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

  .kt-block {
    align-items: center;
  }

  .kt-row {
    justify-content: center;
  }
}

@media (max-width: 768px) {
  .hero-content {
    padding: 0 1.5rem;
  }

  .social-sidebar {
    display: none;
  }

  .name span {
    font-size: 3rem;
  }

  .kt-bg .kt-char {
    font-size: 2.4rem;
  }

  .kt-fg .kt-char {
    font-size: 1.8rem;
  }
}

@media (max-width: 480px) {
  .name span {
    font-size: 2.4rem;
  }

  .greeting,
  .role-prefix {
    font-size: 1rem;
  }
}
</style>
