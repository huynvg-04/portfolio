<script setup>
import { ref, onMounted, onUnmounted, nextTick } from 'vue';
import gsap from 'gsap';
import ParticleBackground from './ParticleBackground.vue';
import ScatterText from './ScatterText.vue';

const roles = [
  { top: 'FULL-STACK', bottom: 'DEVELOPER' }
];

const currentIndex = ref(0);
const incomingRole = ref(roles[0]);
const outgoingRole = ref(null);
let loopId = null;

async function animateTicker() {
  outgoingRole.value = incomingRole.value;
  currentIndex.value = (currentIndex.value + 1) % roles.length;
  incomingRole.value = roles[currentIndex.value];

  await nextTick();

  const tl = gsap.timeline({
    onComplete: () => {
      outgoingRole.value = null;
    }
  });

  const STAGGER_TIME = 0.1;
  const ANIM_DURATION = 1.2;

  tl.to('.outgoing-word.top-row .kt-char', { y: -80, stagger: STAGGER_TIME, duration: ANIM_DURATION, ease: "power3.inOut" }, 0);
  tl.fromTo('.incoming-word.top-row .kt-char',
    { y: 80 },
    { y: 0, stagger: STAGGER_TIME, duration: ANIM_DURATION, ease: "power3.inOut" },
    0
  );


  tl.to('.outgoing-word.bottom-row .kt-char', { y: -80, stagger: STAGGER_TIME, duration: ANIM_DURATION, ease: "power3.inOut" }, 0.2);
  tl.fromTo('.incoming-word.bottom-row .kt-char',
    { y: 80 },
    { y: 0, stagger: STAGGER_TIME, duration: ANIM_DURATION, ease: "power3.inOut" },
    0.2
  );
}

onMounted(() => {
  loopId = setInterval(() => {
    animateTicker();
  }, 4000);
});

onUnmounted(() => {
  if (loopId) clearInterval(loopId);
});
</script>

<template>
  <section id="hero" class="hero-section">
    <ParticleBackground />
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

      <div class="hero-left" data-delay="200">
        <span class="greeting">Hello! I'm</span>
        <h1 class="name">
          <ScatterText text="NGO VAN" />
          <ScatterText text="GIA HUY" />
        </h1>
      </div>

      <div class="hero-right fade-left visible" data-delay="300">
        <span class="role-prefix">A Creative</span>
        <div class="kt-block">
          <div class="kt-row kt-bg">
            <div v-if="outgoingRole" class="word-wrapper outgoing-word top-row">
              <span v-for="(char, i) in outgoingRole.top.split('')" :key="`out-${i}`" class="kt-char"
                v-html="char === ' ' ? '&nbsp;' : char"></span>
            </div>
            <div v-if="incomingRole" class="word-wrapper incoming-word top-row">
              <span v-for="(char, i) in incomingRole.top.split('')" :key="`in-${i}`" class="kt-char"
                v-html="char === ' ' ? '&nbsp;' : char"></span>
            </div>
          </div>
          <div class="kt-row kt-fg">
            <div v-if="outgoingRole" class="word-wrapper outgoing-word bottom-row">
              <span v-for="(char, i) in outgoingRole.bottom.split('')" :key="`out-${i}`" class="kt-char"
                v-html="char === ' ' ? '&nbsp;' : char"></span>
            </div>
            <div v-if="incomingRole" class="word-wrapper incoming-word bottom-row">
              <span v-for="(char, i) in incomingRole.bottom.split('')" :key="`in-${i}`" class="kt-char"
                v-html="char === ' ' ? '&nbsp;' : char"></span>
            </div>
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
  position: fixed;
  left: 1rem;
  bottom: 3rem;
  top: auto;
  transform: none;
  display: flex;
  flex-direction: column;
  gap: 2rem;
  z-index: 9000;
  padding: 1.5rem 1rem;
  border-radius: 50px;
  transition: all 0.2s ease-out;
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

.social-sidebar:hover {
  background: rgb(226, 198, 255);
  box-shadow: 0 0 30px 5px rgba(214, 173, 255, 0.8);
}

.social-sidebar:hover a {
  color: var(--bg-dark);
}

@keyframes smooth-shake {

  0%,
  100% {
    transform: scale(1.15) translate(0, 0) rotate(0deg);
  }

  20% {
    transform: scale(1.15) translate(0.8px, -0.8px) rotate(-2deg);
  }

  40% {
    transform: scale(1.15) translate(-0.8px, 0.8px) rotate(2deg);
  }

  60% {
    transform: scale(1.15) translate(0.8px, 0.8px) rotate(-2deg);
  }

  80% {
    transform: scale(1.15) translate(-0.8px, -0.8px) rotate(2deg);
  }
}

.social-sidebar a:hover {
  animation: smooth-shake 0.4s infinite ease-in-out;
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
  font-family: 'Canela Text', serif;
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

.name>span {
  font-family: 'Canela Text', serif;
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

.word-wrapper {
  display: flex;
  white-space: nowrap;
  overflow: hidden;
}

.outgoing-word {
  position: absolute;
  right: 0;
  pointer-events: none;
}

.kt-fg {
  margin-top: -2em;
  z-index: 2;
}

.kt-bg {
  z-index: 1;
}

.kt-char {
  display: inline-block;
  font-family: 'Canela Text', serif;
  font-weight: 900;
  text-transform: uppercase;
  letter-spacing: -0.02em;
}

.kt-bg .kt-char {
  font-size: clamp(2.8rem, 5.5vw, 5rem);
  color: #c481ff;
}

.kt-fg .kt-char {
  font-size: clamp(2rem, 4vw, 3.8rem);
  color: #fff;
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
