<script setup>
import { ref, onMounted, onUnmounted } from 'vue';
import gsap from 'gsap';
import { ScrollTrigger } from 'gsap/ScrollTrigger';

gsap.registerPlugin(ScrollTrigger);

const projects = [
  {
    num: '01',
    title: 'E-Commerce Platform',
    category: 'Full-stack',
    tech: 'Laravel, MySQL, HTML, CSS, JavaScript',
    github: 'https://github.com/huynvg-04/greenvibes',
    image: '/greenvibes.png',
    link: 'https://github.com/huynvg-04/greenvibes',
  },
  {
    num: '02',
    title: 'Movie Website',
    category: 'Full-stack',
    tech: 'Laravel, MySQL, HTML, CSS',
    github: 'https://github.com/huynvg-04/moviewebsite',
    image: '/moviewebsite.png',
    link: 'https://github.com/huynvg-04/moviewebsite',
  },
  {
    num: '03',
    title: 'Coming Soon',
    category: '—',
    tech: '—',
    github: null,
    image: null,
    link: null,
  },
  {
    num: '04',
    title: 'Coming Soon',
    category: '—',
    tech: '—',
    github: null,
    image: null,
    link: null,
  },
];

const sectionRef = ref(null);
const flexRef = ref(null);
const isHovered = ref([]);
let ctx = null;

onMounted(() => {
  isHovered.value = projects.map(() => false);

  requestAnimationFrame(() => {
    const boxes = document.querySelectorAll('.work-box');
    if (!boxes.length) return;

    const container = document.querySelector('.work-container-inner');
    const rectLeft = container.getBoundingClientRect().left;
    const firstBox = boxes[0];
    const parentWidth = firstBox.parentElement.getBoundingClientRect().width;
    const padding = parseInt(window.getComputedStyle(firstBox).paddingLeft) / 2;
    const translateX = firstBox.getBoundingClientRect().width * boxes.length - (rectLeft + parentWidth) + padding;

    ctx = gsap.context(() => {
      const tl = gsap.timeline({
        scrollTrigger: {
          trigger: '.work-section',
          start: 'top top',
          end: `+=${translateX}`,
          scrub: true,
          pin: true,
          id: 'work',
        },
      });

      tl.to('.work-flex', {
        x: -translateX,
        ease: 'none',
      });
    });
  });
});

onUnmounted(() => {
  if (ctx) ctx.revert();
  ScrollTrigger.getById('work')?.kill();
});

const handleMouseEnter = (index) => {
  isHovered.value[index] = true;
};

const handleMouseLeave = (index) => {
  isHovered.value[index] = false;
};
</script>

<template>
  <section id="projects" class="work-section" ref="sectionRef">
    <div class="work-container-inner section-container">
      <h2>My <span>Work</span></h2>
      <div class="work-flex" ref="flexRef">
        <div v-for="(project, index) in projects" :key="project.num" class="work-box"
          :class="{ even: index % 2 !== 0 }">
          <!-- Info block -->
          <div class="work-info">
            <div class="work-title">
              <h3>{{ project.num }}</h3>
              <div>
                <h4>{{ project.title }}</h4>
                <p>{{ project.category }}</p>
              </div>
            </div>
            <h4 class="tech-label">Tools and features</h4>
            <p class="tech-text">{{ project.tech }}</p>
          </div>

          <div class="work-image">
            <a class="work-image-in" :href="project.link || '#'" :target="project.link ? '_blank' : '_self'"
              rel="noopener" @mouseenter="handleMouseEnter(index)" @mouseleave="handleMouseLeave(index)">
              <template v-if="project.image">
                <div v-if="project.link" class="work-link">↗</div>
                <img :src="project.image" :alt="project.title" />
              </template>
              <template v-else>
                <div class="work-placeholder">
                  <span class="coming-soon-badge">Coming Soon</span>
                  <div class="placeholder-lines">
                    <div class="ph-line" style="width: 70%"></div>
                    <div class="ph-line" style="width: 45%"></div>
                    <div class="ph-line" style="width: 60%"></div>
                  </div>
                </div>
              </template>
            </a>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<style scoped>
.work-section {
  height: 100vh;
  box-sizing: border-box;
  will-change: transform;
  background: var(--bg-dark);
}

.work-section h2 {
  margin-top: 80px;
  margin-bottom: 1rem;
  font-size: clamp(2.5rem, 5vw, 4.5rem);
  font-weight: 500;
  line-height: 1;
  padding-left: 0;
}

.work-section h2>span {
  color: var(--accent);
}

.work-container-inner {
  display: flex;
  flex-direction: column;
  height: 100%;
  max-width: none;
  width: 100%;
  padding: 0 60px;
  box-sizing: border-box;
}

.work-flex {
  width: 100%;
  display: flex;
  height: 100%;
  margin-left: -80px;
  padding-right: 120px;
  position: relative;
}

/* Top & bottom border lines spanning the full scroll width */
.work-flex::before,
.work-flex::after {
  content: '';
  width: 50000vw;
  height: 1px;
  background-color: #363636;
  position: absolute;
  left: 50%;
  top: 0;
  transform: translateX(-50%);
}

.work-flex::after {
  top: 100%;
}

.work-box {
  padding: 60px 80px;
  display: flex;
  flex-direction: column;
  width: 550px;
  box-sizing: border-box;
  border-right: 1px solid #363636;
  flex-shrink: 0;
  gap: 40px;
  justify-content: center;
}

/* Even boxes flip the layout */
.work-box.even {
  flex-direction: column-reverse;
}

/* ── Info ── */
.work-title {
  justify-content: space-between;
  display: flex;
  width: 100%;
  margin-bottom: 16px;
  align-items: flex-start;
}

.work-title>div {
  text-align: right;
}

.work-title h3 {
  font-size: clamp(2.5rem, 5vw, 3.5rem);
  line-height: 1;
  margin: 0;
  font-weight: 600;
  color: var(--text-main);
}

.work-info h4 {
  font-size: 1.05rem;
  font-weight: 500;
  margin: 0;
  color: var(--text-main);
}

.work-info p {
  font-weight: 300;
  color: var(--text-muted);
  margin: 0;
  margin-top: 3px;
  font-size: 0.9rem;
}

.tech-label {
  font-size: 0.75rem !important;
  text-transform: uppercase;
  letter-spacing: 0.1em;
  color: var(--text-muted) !important;
  font-weight: 400 !important;
}

.tech-text {
  font-size: 0.95rem;
  color: var(--text-main) !important;
  line-height: 1.5;
  font-weight: 300 !important;
}

/* ── Image ── */
.work-image {
  display: flex;
  width: 100%;
  justify-content: center;
}

.work-image-in {
  position: relative;
  display: block;
  color: inherit;
  text-decoration: none;
}

.work-link {
  position: absolute;
  bottom: 10px;
  right: 10px;
  background-color: var(--bg-dark);
  width: 48px;
  height: 48px;
  border-radius: 50%;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 1.3rem;
  box-shadow: 0 0 12px rgba(255, 255, 255, 0.45), inset 0 0 10px #393939;
  transition: opacity 0.3s ease;
  opacity: 0;
  color: var(--text-main);
  z-index: 2;
}

.work-image-in:hover .work-link {
  opacity: 1;
}

.work-image img {
  max-width: 100%;
  max-height: 320px;
  display: block;
  object-fit: cover;
  object-position: top;
  border: 1px solid #363636;
}

/* ── Placeholder for coming soon ── */
.work-placeholder {
  width: 390px;
  height: 220px;
  border: 1px dashed #363636;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 1.5rem;
}

.coming-soon-badge {
  font-size: 0.8rem;
  color: var(--text-muted);
  letter-spacing: 0.1em;
  text-transform: uppercase;
  border: 1px dashed #444;
  padding: 0.4rem 1rem;
  border-radius: 50px;
  animation: cpulse 2.5s ease-in-out infinite;
}

@keyframes cpulse {

  0%,
  100% {
    opacity: 0.4;
  }

  50% {
    opacity: 1;
  }
}

.placeholder-lines {
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
  width: 70%;
}

.ph-line {
  height: 8px;
  border-radius: 4px;
  background: linear-gradient(90deg,
      rgba(255, 255, 255, 0.04) 25%,
      rgba(255, 255, 255, 0.09) 50%,
      rgba(255, 255, 255, 0.04) 75%);
  background-size: 200% 100%;
  animation: shimmer 1.8s linear infinite;
}

@keyframes shimmer {
  from {
    background-position: 200% 0;
  }

  to {
    background-position: -200% 0;
  }
}

/* ── Responsive ── */
@media (max-width: 1400px) {
  .work-box {
    width: 450px;
    padding: 50px;
  }

  .work-flex {
    margin-left: -50px;
    padding-right: 75px;
  }

  .work-title h3 {
    font-size: 2.2rem;
  }
}

@media (max-height: 900px) {
  .work-image img {
    max-height: 240px;
  }

  .work-box {
    padding-top: 30px;
    padding-bottom: 30px;
    gap: 24px;
  }

  .work-section h2 {
    margin-top: 50px;
  }
}

@media (max-height: 650px) {
  .work-image img {
    max-height: 180px;
  }

  .work-placeholder {
    height: 160px;
  }

  .work-box {
    gap: 16px;
  }
}
</style>
