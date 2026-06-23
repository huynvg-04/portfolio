<script setup>
const projects = [
  {
    title: 'Nền Tảng Thương Mại Điện Tử',
    desc: 'Hệ thống thương mại điện tử quy mô lớn được xây dựng bằng Vue 3, Vite và thanh toán qua Stripe. Trải nghiệm UX mượt mà và tốc độ tải trang cực nhanh.',
    tech: ['Vue', 'Vite', 'Tailwind'],
    icon: 'fa-solid fa-cart-shopping'
  },
  {
    title: 'Bảng Điều Khiển AI',
    desc: 'Giao diện bảng điều khiển phân tích dành cho các nền tảng AI. Hỗ trợ biểu đồ động, giao diện tối (dark mode) và bố cục trực quan dễ sử dụng.',
    tech: ['Vue 3', 'Chart.js', 'SCSS'],
    icon: 'fa-solid fa-chart-pie'
  },
  {
    title: 'Mạng Xã Hội Realtime',
    desc: 'Ứng dụng mạng xã hội thời gian thực cho phép người dùng chia sẻ bài viết, nhắn tin và kết nối theo thời gian thực (Realtime) với WebSockets.',
    tech: ['Vue', 'Firebase', 'Pinia'],
    icon: 'fa-solid fa-users'
  }
];
</script>

<template>
  <section id="projects" class="projects-section">
    <div class="container">
      <h2 class="section-title">Dự Án <span class="text-gradient">Nổi Bật</span></h2>
      <div class="projects-grid">
        <div class="glass-card project-card" v-for="(project, index) in projects" :key="index">
          <div class="project-icon">
            <i :class="project.icon"></i>
          </div>
          <h3 class="project-title">{{ project.title }}</h3>
          <p class="project-desc">{{ project.desc }}</p>
          <div class="project-tech">
            <span v-for="t in project.tech" :key="t" class="tech-tag">{{ t }}</span>
          </div>
          <a href="#" class="view-btn">Xem Dự Án <i class="fa-solid fa-arrow-right"></i></a>
        </div>
      </div>
    </div>
  </section>
</template>

<style scoped>
.projects-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(320px, 1fr));
  gap: 2rem;
}

.project-card {
  display: flex;
  flex-direction: column;
  position: relative;
  overflow: hidden;
}

.project-card::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 4px;
  background: var(--accent-gradient);
  opacity: 0;
  transition: var(--transition);
}

.project-card:hover::before {
  opacity: 1;
}

.project-icon {
  width: 60px;
  height: 60px;
  background: rgba(56, 189, 248, 0.1);
  border-radius: 16px;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 1.5rem;
  color: var(--accent);
  margin-bottom: 1.5rem;
}

:root[data-theme="light"] .project-icon {
  background: rgba(14, 165, 233, 0.1);
}

.project-title {
  font-size: 1.5rem;
  margin-bottom: 1rem;
}

.project-desc {
  color: var(--text-muted);
  margin-bottom: 2rem;
  flex-grow: 1;
}

.project-tech {
  display: flex;
  flex-wrap: wrap;
  gap: 0.5rem;
  margin-bottom: 2rem;
}

.tech-tag {
  font-size: 0.75rem;
  padding: 0.35rem 0.75rem;
  background: var(--bg-glass);
  border: 1px solid var(--border-glass);
  border-radius: 20px;
  color: var(--text-main);
}

.view-btn {
  color: var(--accent);
  text-decoration: none;
  font-weight: 600;
  display: inline-flex;
  align-items: center;
  gap: 0.5rem;
  margin-top: auto;
  transition: var(--transition);
}

.view-btn:hover {
  filter: brightness(1.2);
  gap: 0.75rem;
}
</style>
