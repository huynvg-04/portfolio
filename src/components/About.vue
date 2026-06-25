<script setup>
import { ref, onMounted } from 'vue';

const skills = [
  { name: 'PHP / Laravel',    level: 85, color: '#777bb4' },
  { name: 'Vue.js / Nuxt',    level: 88, color: '#42b883' },
  { name: 'JavaScript / TS',  level: 85, color: '#38bdf8' },
  { name: 'Node.js / Express',level: 75, color: '#a78bfa' },
  { name: 'CSS / Tailwind',   level: 88, color: '#f472b6' },
];

const tech = [
  { icon: 'fa-brands fa-php',      label: 'PHP',      color: '#777bb4' },
  { icon: 'fa-brands fa-laravel',  label: 'Laravel',  color: '#ff2d20' },
  { icon: 'fa-brands fa-vuejs',    label: 'Vue',      color: '#42b883' },
  { icon: 'fa-brands fa-js',       label: 'JS/TS',    color: '#f7df1e' },
  { icon: 'fa-brands fa-node-js',  label: 'Node',     color: '#68a063' },
  { icon: 'fa-brands fa-git-alt',  label: 'Git',      color: '#f05032' },
  { icon: 'fa-solid fa-database',  label: 'SQL',      color: '#fb923c' },
  { icon: 'fa-brands fa-figma',    label: 'Figma',    color: '#a259ff' },
];

const cards = [
  {
    icon:  'fa-solid fa-wand-magic-sparkles',
    color: '--neon-pink',
    title: 'Đam Mê Thiết Kế',
    text:  'Tôi tin rằng một thiết kế tuyệt vời không chỉ nằm ở vẻ bề ngoài mà còn ở cảm giác mang lại. Mỗi pixel đều được chăm chút để tạo nên trải nghiệm người dùng hoàn hảo.',
  },
  {
    icon:  'fa-brands fa-vuejs',
    color: '#42b883',
    title: 'Chuyên Gia Vue',
    text:  'Thành thạo Vue 3, Composition API, Pinia và Vite. Tôi chuyên xây dựng SPA có khả năng mở rộng, dễ bảo trì và tối ưu hiệu suất cao nhất.',
  },
  {
    icon:  'fa-solid fa-bolt',
    color: '--neon-orange',
    title: 'Tối Ưu Tốc Độ',
    text:  'Tốc độ là một tính năng thiết yếu. Tôi đảm bảo ứng dụng tải cực nhanh và hoạt động trơn tru trên mọi thiết bị và điều kiện mạng.',
  },
];

const skillsFilled = ref(skills.map(() => 0));
const animated     = ref(false);

const animateSkills = () => {
  if (animated.value) return;
  animated.value = true;
  skills.forEach((s, i) => {
    const start = Date.now();
    const duration = 900 + i * 80;
    const tick = () => {
      const elapsed = Date.now() - start;
      const progress = Math.min(elapsed / duration, 1);
      // easeOutCubic
      const eased = 1 - Math.pow(1 - progress, 3);
      skillsFilled.value[i] = Math.round(s.level * eased);
      if (progress < 1) requestAnimationFrame(tick);
    };
    requestAnimationFrame(tick);
  });
};

onMounted(() => {
  const obs = new IntersectionObserver((entries) => {
    if (entries[0].isIntersecting) { animateSkills(); obs.disconnect(); }
  }, { threshold: 0.3 });
  const el = document.getElementById('about');
  if (el) obs.observe(el);
});
</script>

<template>
  <section id="about" class="about-section">
    <div class="container">
      <h2 class="section-title flip-in-bottom">Về <span class="text-gradient">Tôi</span></h2>
      <p class="section-subtitle flip-in-bottom" data-flip-delay="150">
        Một lập trình viên mới tốt nghiệp đam mê kiến tạo những sản phẩm số chất lượng,
        kết hợp kỹ thuật vững chắc với tư duy giải quyết vấn đề sáng tạo.
      </p>

      <!-- Cards -->
      <div class="about-grid">
        <div class="glass-card about-card flip-in-bottom" v-for="(card, i) in cards" :key="i"
             :data-flip-delay="i * 150"
             :style="{ '--card-accent': card.color.startsWith('--') ? `var(${card.color})` : card.color }">
          <div class="card-icon-wrap">
            <i :class="card.icon" :style="{ color: card.color.startsWith('--') ? `var(${card.color})` : card.color }"></i>
          </div>
          <h3>{{ card.title }}</h3>
          <p>{{ card.text }}</p>
        </div>
      </div>

      <!-- Skills + Tech -->
      <div class="skills-tech-grid">
        <!-- Skills Bars -->
        <div class="glass-card skills-card flip-in-left">
          <h3 class="sub-heading">
            <i class="fa-solid fa-chart-bar"></i> Kỹ Năng Chuyên Môn
          </h3>
          <div class="skills-list">
            <div class="skill-item" v-for="(s, i) in skills" :key="i">
              <div class="skill-meta">
                <span class="skill-name">{{ s.name }}</span>
                <span class="skill-pct">{{ skillsFilled[i] }}%</span>
              </div>
              <div class="skill-bar-track">
                <div class="skill-bar-fill"
                     :style="{ width: skillsFilled[i] + '%', background: s.color + ' linear-gradient(90deg, ' + s.color + '99, ' + s.color + ')' }">
                </div>
              </div>
            </div>
          </div>
        </div>

        <!-- Tech Stack -->
        <div class="glass-card tech-card flip-in-right" data-flip-delay="150">
          <h3 class="sub-heading">
            <i class="fa-solid fa-layer-group"></i> Tech Stack
          </h3>
          <div class="tech-grid">
            <div class="tech-item" v-for="t in tech" :key="t.label">
              <div class="tech-icon" :style="{ '--t-color': t.color }">
                <i :class="t.icon" :style="{ color: t.color }"></i>
              </div>
              <span>{{ t.label }}</span>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<style scoped>
.about-section { background: transparent; }

/* ── Cards ── */
.about-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(260px, 1fr));
  gap: 1.5rem;
  margin-bottom: 2rem;
}

.about-card {
  border-top: 3px solid var(--card-accent, var(--accent));
  transition: var(--transition);
}

.about-card:hover {
  box-shadow: 0 0 30px -8px var(--card-accent, var(--shadow-glow));
}

.card-icon-wrap {
  width: 52px;
  height: 52px;
  border-radius: 14px;
  background: var(--bg-glass);
  border: 1px solid var(--border-glass);
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 1.4rem;
  margin-bottom: 1.25rem;
}

.about-card h3 {
  font-size: 1.25rem;
  margin-bottom: 0.75rem;
}

.about-card p {
  color: var(--text-muted);
  line-height: 1.7;
  font-size: 0.95rem;
}

/* ── Skills & Tech Grid ── */
.skills-tech-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 1.5rem;
}

.sub-heading {
  font-size: 1.1rem;
  margin-bottom: 1.75rem;
  display: flex;
  align-items: center;
  gap: 0.6rem;
  color: var(--text-main);
}

.sub-heading i {
  color: var(--accent);
}

/* ── Skills Bars ── */
.skills-list { display: flex; flex-direction: column; gap: 1.2rem; }

.skill-meta {
  display: flex;
  justify-content: space-between;
  margin-bottom: 0.5rem;
}

.skill-name { font-weight: 500; font-size: 0.9rem; }
.skill-pct  { font-size: 0.85rem; color: var(--text-muted); font-family: var(--font-code); }

.skill-bar-track {
  height: 6px;
  background: var(--bg-glass-hover);
  border-radius: 6px;
  overflow: hidden;
}

.skill-bar-fill {
  height: 100%;
  border-radius: 6px;
  transition: width 0.9s cubic-bezier(0.4, 0, 0.2, 1);
  position: relative;
}

.skill-bar-fill::after {
  content: '';
  position: absolute;
  right: 0;
  top: 0;
  width: 6px;
  height: 100%;
  border-radius: 50%;
  background: #fff;
  opacity: 0.6;
}

/* ── Tech Grid ── */
.tech-grid {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 1rem;
}

.tech-item {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0.5rem;
  font-size: 0.75rem;
  color: var(--text-muted);
  font-weight: 500;
}

.tech-icon {
  width: 50px;
  height: 50px;
  border-radius: 12px;
  background: var(--bg-glass);
  border: 1px solid var(--border-glass);
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 1.4rem;
  transition: var(--transition);
}

.tech-item:hover .tech-icon {
  background: var(--accent-dim);
  border-color: var(--t-color, var(--accent));
  transform: translateY(-4px);
  box-shadow: 0 8px 20px rgba(0,0,0,0.2);
}

/* ── Responsive ── */
/* Tablet ≤ 900px */
@media (max-width: 900px) {
  .skills-tech-grid { grid-template-columns: 1fr; }
  .tech-grid { grid-template-columns: repeat(4, 1fr); }
}

/* Mobile ≤ 768px */
@media (max-width: 768px) {
  .about-grid { grid-template-columns: 1fr; gap: 1.25rem; }
  .tech-grid  { grid-template-columns: repeat(4, 1fr); gap: 0.75rem; }
  .tech-icon  { width: 44px; height: 44px; font-size: 1.25rem; }
  .sub-heading { font-size: 1rem; margin-bottom: 1.5rem; }
}

/* Small Mobile ≤ 480px */
@media (max-width: 480px) {
  .about-grid { gap: 1rem; }
  .tech-grid  { grid-template-columns: repeat(4, 1fr); gap: 0.6rem; }
  .tech-icon  { width: 40px; height: 40px; font-size: 1.1rem; border-radius: 10px; }
  .tech-item  { font-size: 0.7rem; gap: 0.35rem; }
  .card-icon-wrap { width: 46px; height: 46px; font-size: 1.25rem; }
  .about-card h3  { font-size: 1.1rem; }
  .about-card p   { font-size: 0.88rem; }
  .skill-name { font-size: 0.85rem; }
  .skill-pct  { font-size: 0.8rem; }
}

/* Very Small ≤ 360px */
@media (max-width: 360px) {
  .tech-grid { grid-template-columns: repeat(4, 1fr); gap: 0.5rem; }
  .tech-icon { width: 36px; height: 36px; font-size: 1rem; border-radius: 8px; }
  .tech-item { font-size: 0.65rem; }
}
</style>
