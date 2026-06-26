<script setup>
import { ref, computed } from 'vue';

const filters = ['Tất Cả', 'Frontend', 'Fullstack', 'Backend'];
const activeFilter = ref('Tất Cả');

const projects = [
  {
    title: 'Nền Tảng Thương Mại Điện Tử',
    desc:  'Hệ thống thương mại điện tử quy mô lớn với thanh toán Stripe, giỏ hàng real-time, và bảng quản trị admin toàn diện. UX mượt mà, tốc độ cực nhanh.',
    tech:  ['PHP', 'Laravel', 'MySQL'],
    icon:  'fa-solid fa-cart-shopping',
    color: '#38bdf8',
    category: 'Fullstack',
    github: 'https://github.com/huynvg-04/greenvibes',
    demo:   '#',
  }
];

const filtered = computed(() =>
  activeFilter.value === 'Tất Cả'
    ? projects
    : projects.filter(p => p.category === activeFilter.value)
);

const handleMouseMove = (e, card) => {
  const rect   = card.getBoundingClientRect();
  const x      = e.clientX - rect.left;
  const y      = e.clientY - rect.top;
  const cx     = rect.width  / 2;
  const cy     = rect.height / 2;
  const rotateX =  ((y - cy) / cy) * -6;
  const rotateY =  ((x - cx) / cx) *  6;
  card.style.transform = `perspective(800px) rotateX(${rotateX}deg) rotateY(${rotateY}deg) translateY(-8px)`;
};
const handleMouseLeave = (card) => {
  card.style.transform = '';
};
</script>

<template>
  <section id="projects" class="projects-section">
    <div class="bg-glow-2 bg-glow" style="opacity:0.07;"></div>
    <div class="container">
      <h2 class="section-title flip-in-right">Dự Án <span class="text-gradient">Nổi Bật</span></h2>
      <p class="section-subtitle flip-in-right" data-flip-delay="150">Một số sản phẩm tôi đã xây dựng — từ giao diện đến kiến trúc hệ thống.</p>

      <div class="filter-tabs flip-in-right" data-flip-delay="200">
        <button
          v-for="f in filters" :key="f"
          :class="['filter-btn', { active: activeFilter === f }]"
          @click="activeFilter = f">
          {{ f }}
        </button>
      </div>

      <div class="projects-grid">
        <div
          class="glass-card project-card flip-in-bottom"
          v-for="(project, index) in filtered"
          :key="project.title"
          :data-flip-delay="index * 150"
          :style="{ '--p-color': project.color }"
          @mousemove="handleMouseMove($event, $event.currentTarget)"
          @mouseleave="handleMouseLeave($event.currentTarget)"
        >
          <div class="project-top-line"></div>

          <div class="project-header">
            <div class="project-icon-wrap">
              <i :class="project.icon" :style="{ color: project.color }"></i>
            </div>
            <span class="project-category">{{ project.category }}</span>
          </div>

          <h3 class="project-title">{{ project.title }}</h3>
          <p class="project-desc">{{ project.desc }}</p>

          <div class="project-tech">
            <span v-for="t in project.tech" :key="t" class="tech-tag">{{ t }}</span>
          </div>

          <div class="project-links">
            <a :href="project.github" target="_blank" rel="noopener" class="proj-link" id="proj-github">
              <i class="fa-brands fa-github"></i> GitHub
            </a>
            <a :href="project.demo" class="proj-link proj-demo" id="proj-demo">
              <i class="fa-solid fa-arrow-up-right-from-square"></i> Live Demo
            </a>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<style scoped>
.projects-section { overflow: hidden; }

.filter-tabs {
  display: flex;
  justify-content: center;
  gap: 0.75rem;
  margin-bottom: 3rem;
  flex-wrap: wrap;
}

.filter-btn {
  padding: 0.55rem 1.4rem;
  border-radius: 50px;
  border: 1px solid var(--border-glass);
  background: var(--bg-glass);
  color: var(--text-muted);
  font-family: var(--font-main);
  font-weight: 500;
  font-size: 0.9rem;
  cursor: pointer;
  transition: var(--transition);
}

.filter-btn:hover {
  border-color: var(--accent);
  color: var(--accent);
}

.filter-btn.active {
  background: var(--accent-gradient);
  color: #fff;
  border-color: transparent;
  box-shadow: 0 4px 16px var(--shadow-glow);
}

.projects-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(290px, 1fr));
  gap: 1.75rem;
}

.project-card {
  display: flex;
  flex-direction: column;
  padding: 0;
  overflow: hidden;
  transition: transform 0.2s ease, box-shadow 0.2s ease, border-color 0.3s ease !important;
  cursor: default;
}

.project-card:hover {
  border-color: var(--p-color) !important;
  box-shadow: 0 20px 60px rgba(0,0,0,0.3), 0 0 30px -10px var(--p-color) !important;
  transform: none !important;
}

.project-top-line {
  height: 3px;
  background: linear-gradient(90deg, var(--p-color, var(--accent)), transparent);
  width: 100%;
}

.project-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 1.5rem 1.75rem 0;
}

.project-icon-wrap {
  width: 54px;
  height: 54px;
  border-radius: 14px;
  background: var(--bg-glass);
  border: 1px solid var(--border-glass);
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 1.4rem;
}

.project-category {
  font-size: 0.75rem;
  padding: 0.3rem 0.75rem;
  border-radius: 50px;
  background: var(--bg-glass);
  border: 1px solid var(--border-glass);
  color: var(--text-muted);
  font-weight: 500;
}

.project-title {
  font-size: 1.25rem;
  padding: 1.25rem 1.75rem 0.75rem;
  line-height: 1.3;
}

.project-desc {
  color: var(--text-muted);
  font-size: 0.9rem;
  line-height: 1.7;
  padding: 0 1.75rem 1.25rem;
  flex-grow: 1;
}

.project-tech {
  display: flex;
  flex-wrap: wrap;
  gap: 0.5rem;
  padding: 0 1.75rem 1.25rem;
}

.tech-tag {
  font-size: 0.72rem;
  padding: 0.3rem 0.75rem;
  background: var(--bg-glass);
  border: 1px solid var(--border-glass);
  border-radius: 20px;
  color: var(--text-muted);
  font-family: var(--font-code);
}

.project-links {
  display: flex;
  gap: 1rem;
  padding: 1.25rem 1.75rem 1.5rem;
  border-top: 1px solid var(--border-glass);
  margin-top: auto;
}

.proj-link {
  display: inline-flex;
  align-items: center;
  gap: 0.45rem;
  color: var(--text-muted);
  text-decoration: none;
  font-size: 0.87rem;
  font-weight: 500;
  transition: var(--transition-fast);
}

.proj-link:hover {
  color: var(--text-main);
}

.proj-demo {
  margin-left: auto;
  color: var(--p-color, var(--accent));
}

.proj-demo:hover {
  filter: brightness(1.2);
}

@media (max-width: 900px) {
  .projects-grid { grid-template-columns: repeat(auto-fit, minmax(280px, 1fr)); gap: 1.5rem; }
}

@media (max-width: 768px) {
  .projects-grid { grid-template-columns: 1fr; gap: 1.25rem; }
  .project-header { padding: 1.25rem 1.5rem 0; }
  .project-title  { font-size: 1.15rem; padding: 1rem 1.5rem 0.65rem; }
  .project-desc   { font-size: 0.875rem; padding: 0 1.5rem 1rem; }
  .project-tech   { padding: 0 1.5rem 1rem; }
  .project-links  { padding: 1rem 1.5rem 1.25rem; }
  .filter-btn     { padding: 0.5rem 1.1rem; font-size: 0.85rem; }
  .filter-tabs    { gap: 0.5rem; margin-bottom: 2rem; }
  .project-icon-wrap { width: 46px; height: 46px; font-size: 1.25rem; }
}

@media (max-width: 480px) {
  .projects-grid { gap: 1rem; }
  .project-header { padding: 1rem 1.25rem 0; }
  .project-title  { font-size: 1.1rem; padding: 0.875rem 1.25rem 0.6rem; }
  .project-desc   { font-size: 0.85rem; padding: 0 1.25rem 0.875rem; }
  .project-tech   { padding: 0 1.25rem 0.875rem; gap: 0.4rem; }
  .project-links  { padding: 0.875rem 1.25rem 1.125rem; }
  .tech-tag       { font-size: 0.68rem; padding: 0.25rem 0.6rem; }
  .filter-btn     { padding: 0.45rem 0.9rem; font-size: 0.82rem; }
  .filter-tabs    { gap: 0.4rem; margin-bottom: 1.75rem; }
}

@media (max-width: 360px) {
  .filter-tabs { gap: 0.35rem; }
  .filter-btn  { padding: 0.4rem 0.8rem; font-size: 0.8rem; }
  .project-title { font-size: 1.05rem; }
}
</style>
