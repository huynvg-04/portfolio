<script setup>
import { onMounted } from 'vue';

const techStack = [
  { icon: 'fa-brands fa-html5', color: '#e34f26', name: 'HTML5' },
  { icon: 'fa-brands fa-css3-alt', color: '#1572b6', name: 'CSS3' },
  { icon: 'fa-brands fa-js', color: '#f7df1e', name: 'JavaScript' },
  { icon: 'fa-brands fa-vuejs', color: '#42b883', name: 'Vue.js' },
  { icon: 'fa-brands fa-react', color: '#61dafb', name: 'React.js' },
  { icon: 'fa-brands fa-php', color: '#777bb4', name: 'PHP' },
  { icon: 'fa-brands fa-laravel', color: '#ff2d20', name: 'Laravel' },
  { icon: 'fa-brands fa-node-js', color: '#68a063', name: 'Node.js' },
  { icon: 'fa-brands fa-bootstrap', color: '#7952b3', name: 'Bootstrap' },
  { icon: 'fa-solid fa-wind', color: '#38bdf8', name: 'Tailwind CSS' },
  { icon: 'fa-solid fa-database', color: '#fb923c', name: 'SQL' },
  { icon: 'fa-brands fa-git-alt', color: '#f05032', name: 'Git' },
  { icon: 'fa-brands fa-figma', color: '#a259ff', name: 'Figma' },
];

onMounted(() => {
  // Intersection Observer for fade-up animations
  const observer = new IntersectionObserver((entries) => {
    entries.forEach(entry => {
      if (entry.isIntersecting) {
        entry.target.classList.add('in-view');
      }
    });
  }, { threshold: 0.1 });

  document.querySelectorAll('.bento-item').forEach(el => observer.observe(el));
});
</script>

<template>
  <section id="about" class="about-section">
    <div class="container">
      <h2 class="section-title">About <span class="text-gradient">Me</span></h2>
      <p class="section-subtitle">
        Discover my journey, skills, and what makes my work style unique.
      </p>

      <div class="bento-grid">
        
        <!-- ── Card 1: Bio ── -->
        <div class="bento-item glass-card card-bio">
          <div class="bio-content">
            <div class="bio-header">
              <div class="avatar-wrap">
                <!-- Avatar placeholder -->
                <i class="fa-solid fa-user-astronaut"></i>
              </div>
              <div class="bio-title">
                <h3>Hi, I'm <span class="text-gradient">Huy</span> 👋</h3>
                <p class="role">Fullstack Developer</p>
              </div>
            </div>
            <p class="bio-text">
              I'm a full-stack developer focused on building <strong>clean</strong>, <strong>efficient</strong>, and <strong>visually stunning</strong> web applications. <br><br>
              I bridge the gap between intuitive design and robust technical architecture to deliver high-quality digital experiences.
            </p>
          </div>
        </div>

        <!-- ── Card 2: Location ── -->
        <div class="bento-item glass-card card-location">
          <div class="bento-inner">
            <div class="icon-circle">
              <i class="fa-solid fa-earth-asia"></i>
            </div>
            <div class="loc-text">
              <h4>Location</h4>
              <p>Vietnam</p>
              <span class="status-badge"><span class="dot"></span> Open to work</span>
            </div>
          </div>
        </div>

        <!-- ── Card 3: Tech Stack ── -->
        <div class="bento-item glass-card card-tech">
          <h4><i class="fa-solid fa-layer-group"></i> Tech Stack</h4>
          <div class="tech-cloud">
            <div v-for="tech in techStack" :key="tech.name" class="tech-pill" :style="{ '--tc': tech.color }">
              <i :class="tech.icon"></i>
              <span>{{ tech.name }}</span>
            </div>
          </div>
        </div>

        <!-- ── Card 4: Fun Fact ── -->
        <div class="bento-item glass-card card-funfact">
          <div class="bento-inner">
            <div class="icon-circle highlight">
              <i class="fa-solid fa-gamepad"></i>
            </div>
            <div class="fact-text">
              <h4>Hobbies</h4>
              <p>When I'm not coding, you can find me playing games, listening to indie music, or exploring UI/UX Design.</p>
            </div>
          </div>
        </div>

        <!-- ── Card 5: Learning ── -->
        <div class="bento-item glass-card card-learning">
          <h4><i class="fa-solid fa-book-open-reader"></i> Currently Learning</h4>
          <div class="learning-list">
            <div class="learn-item">
              <i class="fa-brands fa-react"></i> React / Next.js
            </div>
            <div class="learn-item">
              <i class="fa-solid fa-server"></i> Microservices Architecture
            </div>
          </div>
        </div>

      </div>
    </div>
  </section>
</template>

<style scoped>
.about-section {
  padding: 6rem 0;
  background: transparent;
}

.bento-grid {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  grid-auto-rows: minmax(180px, auto);
  gap: 1.25rem;
  margin-top: 2.5rem;
}

.bento-item {
  border-radius: 24px;
  padding: 2rem;
  display: flex;
  flex-direction: column;
  transition: transform 0.4s ease, box-shadow 0.4s ease;
  opacity: 0;
  transform: translateY(30px);
}

.bento-item.in-view {
  opacity: 1;
  transform: translateY(0);
}

.bento-item:hover {
  transform: translateY(-5px);
  box-shadow: 0 15px 40px rgba(0,0,0,0.3);
}

.bento-inner {
  display: flex;
  flex-direction: column;
  height: 100%;
  justify-content: center;
}

/* ── Specific Cards ── */
.card-bio {
  grid-column: span 2;
  grid-row: span 2;
  background: linear-gradient(145deg, rgba(255,255,255,0.06), rgba(255,255,255,0.01));
}

.card-location {
  grid-column: span 1;
  grid-row: span 1;
}

.card-tech {
  grid-column: span 2;
  grid-row: span 2;
}

.card-funfact {
  grid-column: span 1;
  grid-row: span 1;
}

.card-learning {
  grid-column: span 2;
  grid-row: span 1;
}

/* ── Bio Content ── */
.bio-header {
  display: flex;
  align-items: center;
  gap: 1.5rem;
  margin-bottom: 1.5rem;
}

.avatar-wrap {
  width: 80px;
  height: 80px;
  border-radius: 50%;
  background: var(--bg-glass-hover);
  border: 2px solid var(--accent);
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 2.5rem;
  color: var(--accent);
}

.bio-title h3 {
  font-size: 1.75rem;
  margin-bottom: 0.25rem;
}

.role {
  color: var(--text-muted);
  font-family: var(--font-code);
  font-size: 0.95rem;
}

.bio-text {
  color: var(--text-muted);
  line-height: 1.8;
  font-size: 1.05rem;
}

.bio-text strong {
  color: var(--text-main);
  font-weight: 600;
}

/* ── Common Utilities ── */
.icon-circle {
  width: 48px;
  height: 48px;
  border-radius: 14px;
  background: var(--bg-glass-hover);
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 1.5rem;
  color: var(--accent);
  margin-bottom: 1rem;
}

.icon-circle.highlight {
  color: #f472b6;
}

h4 {
  font-size: 1.15rem;
  margin-bottom: 0.5rem;
  display: flex;
  align-items: center;
  gap: 0.5rem;
}
h4 i { color: var(--accent); }

/* ── Location ── */
.loc-text p {
  color: var(--text-muted);
  font-size: 0.95rem;
  margin-bottom: 1rem;
}

.status-badge {
  display: inline-flex;
  align-items: center;
  gap: 0.5rem;
  padding: 0.4rem 0.8rem;
  background: rgba(34, 197, 94, 0.1);
  border: 1px solid rgba(34, 197, 94, 0.2);
  color: #4ade80;
  border-radius: 50px;
  font-size: 0.8rem;
  font-weight: 500;
}

.dot {
  width: 8px;
  height: 8px;
  background: #4ade80;
  border-radius: 50%;
  box-shadow: 0 0 10px #4ade80;
  animation: pulse 2s infinite;
}

@keyframes pulse {
  0% { box-shadow: 0 0 0 0 rgba(74, 222, 128, 0.4); }
  70% { box-shadow: 0 0 0 10px rgba(74, 222, 128, 0); }
  100% { box-shadow: 0 0 0 0 rgba(74, 222, 128, 0); }
}

/* ── Tech Cloud ── */
.tech-cloud {
  display: flex;
  flex-wrap: wrap;
  gap: 0.75rem;
  margin-top: 1.25rem;
}

.tech-pill {
  display: inline-flex;
  align-items: center;
  gap: 0.5rem;
  padding: 0.6rem 1rem;
  background: var(--bg-glass);
  border: 1px solid var(--border-glass);
  border-radius: 12px;
  font-size: 0.9rem;
  font-weight: 500;
  color: var(--text-main);
  transition: var(--transition);
  cursor: default;
}

.tech-pill i {
  color: var(--tc);
  font-size: 1.1rem;
}

.tech-pill:hover {
  background: var(--bg-glass-hover);
  border-color: var(--tc);
  transform: translateY(-3px);
  box-shadow: 0 5px 15px rgba(0,0,0,0.2);
}

/* ── Fun Fact ── */
.fact-text p {
  color: var(--text-muted);
  line-height: 1.6;
  font-size: 0.95rem;
}

/* ── Learning ── */
.learning-list {
  display: flex;
  flex-direction: column;
  gap: 0.85rem;
  margin-top: 1rem;
}

.learn-item {
  display: flex;
  align-items: center;
  gap: 0.75rem;
  padding: 1rem;
  background: var(--bg-glass);
  border-radius: 12px;
  color: var(--text-muted);
  font-weight: 500;
}

.learn-item i {
  color: var(--accent);
  font-size: 1.2rem;
}

/* ── Responsive ── */
@media (max-width: 1024px) {
  .bento-grid {
    grid-template-columns: repeat(2, 1fr);
  }
  .card-location { grid-column: span 1; }
  .card-funfact { grid-column: span 1; }
}

@media (max-width: 640px) {
  .bento-grid {
    grid-template-columns: 1fr;
  }
  .card-bio, .card-tech, .card-learning, .card-location, .card-funfact {
    grid-column: span 1;
    grid-row: auto;
  }
  .bento-item { padding: 1.5rem; }
  .bio-header { flex-direction: column; align-items: flex-start; gap: 1rem; }
}
</style>
