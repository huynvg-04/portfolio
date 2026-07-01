<script setup>
import { onMounted, ref } from 'vue';

const sectionRef = ref(null);

const techStack = [
  { icon: 'fa-brands fa-html5',    color: '#e34f26', name: 'HTML5' },
  { icon: 'fa-brands fa-css3-alt', color: '#1572b6', name: 'CSS3' },
  { icon: 'fa-brands fa-js',       color: '#f7df1e', name: 'JavaScript' },
  { icon: 'fa-brands fa-vuejs',    color: '#42b883', name: 'Vue.js' },
  { icon: 'fa-brands fa-react',    color: '#61dafb', name: 'React.js' },
  { icon: 'fa-brands fa-php',      color: '#777bb4', name: 'PHP' },
  { icon: 'fa-brands fa-laravel',  color: '#ff2d20', name: 'Laravel' },
  { icon: 'fa-brands fa-node-js',  color: '#68a063', name: 'Node.js' },
  { icon: 'fa-solid fa-wind',      color: '#38bdf8', name: 'Tailwind' },
  { icon: 'fa-solid fa-database',  color: '#fb923c', name: 'SQL' },
  { icon: 'fa-brands fa-git-alt',  color: '#f05032', name: 'Git' },
  { icon: 'fa-brands fa-figma',    color: '#a259ff', name: 'Figma' },
];

onMounted(() => {
  const observer = new IntersectionObserver((entries) => {
    entries.forEach(entry => {
      if (entry.isIntersecting) {
        entry.target.classList.add('visible');
        observer.unobserve(entry.target);
      }
    });
  }, { threshold: 0.12, rootMargin: '0px 0px -60px 0px' });

  sectionRef.value?.querySelectorAll('.fade-up, .fade-left, .fade-right, .fade-blur').forEach(el => observer.observe(el));
});
</script>

<template>
  <section id="about" class="about-section" ref="sectionRef">
    <div class="about-inner container">

      <!-- Left: label -->
      <div class="about-label fade-left" data-delay="0">
        <span class="about-tag">/ ABOUT ME</span>
      </div>

      <!-- Right: content -->
      <div class="about-content">
        <h3 class="about-title fade-up" data-delay="100">
          Passionate about crafting<br />
          <span class="text-gradient">digital experiences</span>
        </h3>

        <p class="about-bio fade-up" data-delay="200">
          I'm a Full-Stack Developer focused on building clean, performant, and
          visually compelling web applications. With a strong foundation in both
          frontend and backend technologies, I love turning complex problems into
          elegant, user-friendly solutions.
        </p>
        <p class="about-bio fade-up" data-delay="300">
          When I'm not coding, I'm exploring new technologies, sharpening my
          design skills in Figma, or contributing to open-source projects. I
          believe great software is built at the intersection of engineering and creativity.
        </p>

        <div class="about-stats fade-up" data-delay="400">
          <div class="stat-item">
            <span class="stat-num">2+</span>
            <span class="stat-label">Years Experience</span>
          </div>
          <div class="stat-divider"></div>
          <div class="stat-item">
            <span class="stat-num">10+</span>
            <span class="stat-label">Projects Built</span>
          </div>
          <div class="stat-divider"></div>
          <div class="stat-item">
            <span class="stat-num">5+</span>
            <span class="stat-label">Technologies</span>
          </div>
        </div>

        <div class="tech-stack fade-up" data-delay="500">
          <p class="tech-stack-label">Tech Stack</p>
          <div class="tech-grid">
            <div
              v-for="tech in techStack"
              :key="tech.name"
              class="tech-item"
              :style="{ '--tech-color': tech.color }"
            >
              <i :class="tech.icon"></i>
              <span>{{ tech.name }}</span>
            </div>
          </div>
        </div>
      </div>

    </div>
  </section>
</template>

<style scoped>
.about-section {
  padding: 8rem 0 6rem;
  position: relative;
}

.about-inner {
  display: flex;
  align-items: flex-start;
  gap: 6rem;
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 2rem;
}

/* ── Left label column ── */
.about-label {
  flex-shrink: 0;
  padding-top: 0.5rem;
}

.about-tag {
  font-family: var(--font-code);
  font-size: 0.78rem;
  letter-spacing: 0.18em;
  color: var(--accent);
  text-transform: uppercase;
  writing-mode: vertical-rl;
  transform: rotate(180deg);
  opacity: 0.7;
}

/* ── Right content column ── */
.about-content {
  flex: 1;
  max-width: 700px;
}

.about-title {
  font-size: clamp(2rem, 4vw, 3rem);
  font-weight: 700;
  line-height: 1.2;
  margin-bottom: 2rem;
  color: var(--text-main);
}

.about-bio {
  font-size: 1.05rem;
  color: var(--text-muted);
  line-height: 1.85;
  font-weight: 300;
  margin-bottom: 1.4rem;
  letter-spacing: 0.01em;
}

/* ── Stats ── */
.about-stats {
  display: flex;
  align-items: center;
  gap: 2.5rem;
  margin-top: 2.5rem;
  padding-top: 2rem;
  border-top: 1px solid var(--border-glass);
}

.stat-item {
  display: flex;
  flex-direction: column;
  gap: 0.3rem;
}

.stat-num {
  font-size: 2.2rem;
  font-weight: 700;
  background: var(--accent-gradient);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
  line-height: 1;
}

.stat-label {
  font-size: 0.78rem;
  color: var(--text-muted);
  text-transform: uppercase;
  letter-spacing: 0.1em;
  font-weight: 400;
}

.stat-divider {
  width: 1px;
  height: 40px;
  background: var(--border-glass);
  flex-shrink: 0;
}

.tech-stack {
  margin-top: 3rem;
  padding-top: 2.5rem;
  border-top: 1px solid var(--border-glass);
}

.tech-stack-label {
  font-family: var(--font-code);
  font-size: 0.72rem;
  letter-spacing: 0.18em;
  text-transform: uppercase;
  color: var(--text-muted);
  opacity: 0.5;
  margin-bottom: 1.2rem;
}

.tech-grid {
  display: flex;
  flex-wrap: wrap;
  gap: 0.75rem;
}

.tech-item {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  padding: 0.45rem 1rem;
  border: 1px solid var(--border-glass);
  border-radius: 50px;
  background: var(--bg-glass);
  font-size: 0.88rem;
  color: var(--text-muted);
  cursor: default;
  transition: all 0.25s ease;
  white-space: nowrap;
}

.tech-item i {
  font-size: 1rem;
  color: var(--tech-color);
  transition: transform 0.25s ease;
}

.tech-item:hover {
  background: var(--bg-glass-hover);
  border-color: var(--tech-color);
  color: var(--text-main);
  box-shadow: 0 0 12px color-mix(in srgb, var(--tech-color) 30%, transparent);
  transform: translateY(-2px);
}

.tech-item:hover i {
  transform: scale(1.2);
}

/* ── Responsive ── */
@media (max-width: 900px) {
  .about-inner {
    flex-direction: column;
    gap: 2rem;
  }

  .about-tag {
    writing-mode: horizontal-tb;
    transform: none;
  }

  .about-stats {
    gap: 1.5rem;
  }
}

@media (max-width: 600px) {
  .about-section {
    padding: 5rem 0 4rem;
  }

  .about-stats {
    flex-wrap: wrap;
    gap: 1.2rem;
  }

  .stat-divider {
    display: none;
  }
}
</style>
