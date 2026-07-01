<script setup>
import { ref, onMounted, onUnmounted } from 'vue';
import gsap from 'gsap';
import { ScrollTrigger } from 'gsap/ScrollTrigger';

gsap.registerPlugin(ScrollTrigger);

const lineRef = ref(null);
const dotRef = ref(null);
const bodyRef = ref(null);

const experiences = [
  {
    year: '2019',
    role: 'THPT Hoài Đức B, Hà Nội ',
    company: 'A11 - K42',
    desc: 'Started building web applications with HTML, CSS, and JavaScript. Learned Vue.js and began creating responsive, interactive interfaces for personal and academic projects.',
  },
  {
    year: '2022',
    role: 'Ha Noi University of Natural Resources and Environment',
    company: 'Information Technology ĐH12C4',
    desc: 'Developed full-stack applications using Laravel, MySQL, and Vue.js. Built an e-commerce platform (GreenVibes) and a movie website, gaining deep backend experience with REST APIs.',
  },
  {
    year: '2026',
    role: 'Full-Stack Developer',
    company: 'Freelance & Open Source',
    desc: 'Building production-grade web applications with modern stacks including Vue.js, React, Node.js and Laravel. Focused on performance, clean architecture and compelling UI/UX.',
  },
];

let ctx = null;

onMounted(() => {
  ctx = gsap.context(() => {
    gsap.fromTo(
      lineRef.value,
      { height: '0%' },
      {
        height: '100%',
        ease: 'none',
        scrollTrigger: {
          trigger: bodyRef.value,
          start: 'top 80%',
          end: 'bottom 60%',
          scrub: 0.6,
        },
      }
    );

    ScrollTrigger.create({
      trigger: bodyRef.value,
      start: 'top 80%',
      end: 'bottom 60%',
      scrub: 0.6,
      onUpdate: (self) => {
        if (!bodyRef.value || !dotRef.value) return;
        const totalH = bodyRef.value.getBoundingClientRect().height;
        const filled = totalH * self.progress;
        dotRef.value.style.top = `${filled}px`;
        dotRef.value.style.bottom = 'auto';
        dotRef.value.style.transform = 'translateX(-50%) translateY(-50%)';
      },
    });
  });
});

onUnmounted(() => {
  ctx?.revert();
});
</script>

<template>
  <section id="career" class="career-section">
    <div class="career-inner container">

      <div class="career-header">
        <h2 class="career-title fade-up" data-delay="0">
          My career
          <span class="career-amp">&amp;</span>
          <br />experience
        </h2>
      </div>

      <div class="career-body" ref="bodyRef">
        <div class="career-line">
          <div class="career-line-fill" ref="lineRef"></div>
          <div class="career-dot" ref="dotRef"></div>
        </div>

        <div class="career-entries">
          <div v-for="(exp, i) in experiences" :key="i" class="career-entry fade-up" :data-delay="i * 150">
            <div class="career-entry-left">
              <div class="career-meta">
                <h4 class="career-role">{{ exp.role }}</h4>
                <h5 class="career-company">{{ exp.company }}</h5>
              </div>
              <h3 class="career-year">{{ exp.year }}</h3>
            </div>
            <p class="career-desc">{{ exp.desc }}</p>
          </div>
        </div>
      </div>

    </div>
  </section>
</template>

<style scoped>
.career-section {
  padding: 8rem 0 10rem;
  position: relative;
}

.career-inner {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 2rem;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.career-header {
  text-align: center;
  margin-bottom: 6rem;
}

.career-title {
  font-size: clamp(2.8rem, 6vw, 5rem);
  font-weight: 400;
  line-height: 1.05;
  text-align: center;
  background: linear-gradient(180deg, #ffffff 0%, var(--accent) 100%);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

.career-amp {
  font-family: var(--font-code);
  font-weight: 300;
  opacity: 0.7;
}

.career-body {
  position: relative;
  width: 100%;
  display: flex;
  gap: 4rem;
}

.career-line {
  flex-shrink: 0;
  width: 3px;
  position: relative;
  align-self: stretch;
}

.career-line-fill {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 0%;
  background: linear-gradient(to bottom,
      transparent 0%,
      var(--accent) 30%,
      #7c3aed 100%);
  border-radius: 2px;
}

.career-dot {
  position: absolute;
  top: 0;
  left: 50%;
  transform: translateX(-50%) translateY(-50%);
  width: 12px;
  height: 12px;
  border-radius: 50%;
  background: var(--accent);
  box-shadow:
    0 0 8px 3px rgba(194, 164, 255, 0.6),
    0 0 20px 8px rgba(194, 164, 255, 0.3),
    0 0 60px 16px rgba(194, 164, 255, 0.15);
  animation: dot-pulse 1.8s ease-in-out infinite;
  will-change: top;
  z-index: 2;
}

@keyframes dot-pulse {

  0%,
  100% {
    box-shadow:
      0 0 8px 3px rgba(194, 164, 255, 0.5),
      0 0 20px 8px rgba(194, 164, 255, 0.2);
  }

  50% {
    box-shadow:
      0 0 12px 5px rgba(194, 164, 255, 0.8),
      0 0 32px 14px rgba(194, 164, 255, 0.4),
      0 0 80px 24px rgba(194, 164, 255, 0.2);
  }
}

.career-entries {
  flex: 1;
  display: flex;
  flex-direction: column;
  gap: 3.5rem;
}

.career-entry {
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
  gap: 3rem;
  padding-bottom: 3.5rem;
  border-bottom: 1px solid var(--border-glass);
}

.career-entry:last-child {
  border-bottom: none;
  padding-bottom: 0;
}

.career-entry-left {
  display: flex;
  align-items: flex-start;
  gap: 2.5rem;
  flex-shrink: 0;
  width: 42%;
}

.career-meta {
  flex: 1;
}

.career-role {
  font-size: 1.3rem;
  font-weight: 600;
  line-height: 1.3;
  color: var(--text-main);
  margin-bottom: 0.4rem;
}

.career-company {
  font-size: 0.95rem;
  font-weight: 400;
  color: var(--accent);
  letter-spacing: 0.03em;
}

.career-year {
  font-size: 2.8rem;
  font-weight: 700;
  line-height: 1;
  color: var(--text-main);
  font-variant-numeric: tabular-nums;
  flex-shrink: 0;
  opacity: 0.85;
}

.career-desc {
  flex: 1;
  font-size: 1rem;
  font-weight: 300;
  color: var(--text-muted);
  line-height: 1.8;
  letter-spacing: 0.01em;
  padding-top: 0.2rem;
}

@media (max-width: 900px) {
  .career-body {
    flex-direction: column;
    gap: 0;
  }

  .career-line {
    width: 100%;
    height: 3px;
    align-self: auto;
    margin-bottom: 3rem;
  }

  .career-line-fill {
    width: 0%;
    height: 100%;
    background: linear-gradient(to right, transparent 0%, var(--accent) 50%, #7c3aed 100%);
    top: 0;
    left: 0;
  }

  .timeline-animate .career-line-fill {
    width: 100%;
    height: 100%;
  }

  .career-dot {
    right: 0;
    left: auto;
    top: 50%;
    bottom: auto;
    transform: translateY(-50%) translateX(50%);
  }

  .career-entry {
    flex-direction: column;
    gap: 1rem;
  }

  .career-entry-left {
    width: 100%;
  }
}

@media (max-width: 600px) {
  .career-section {
    padding: 5rem 0 7rem;
  }

  .career-header {
    margin-bottom: 4rem;
  }

  .career-entry-left {
    gap: 1.5rem;
  }

  .career-year {
    font-size: 2rem;
  }

  .career-role {
    font-size: 1.1rem;
  }
}
</style>
