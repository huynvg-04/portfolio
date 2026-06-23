<script setup>
import { ref, onMounted } from 'vue';

const isScrolled = ref(false);
const theme = ref('dark');

const toggleTheme = () => {
  theme.value = theme.value === 'dark' ? 'light' : 'dark';
  document.documentElement.setAttribute('data-theme', theme.value);
};

onMounted(() => {
  window.addEventListener('scroll', () => {
    isScrolled.value = window.scrollY > 50;
  });
  
  // Set default theme from os/browser preference if available
  const prefersLight = window.matchMedia && window.matchMedia('(prefers-color-scheme: light)').matches;
  if (prefersLight) {
    theme.value = 'light';
    document.documentElement.setAttribute('data-theme', 'light');
  }
});
</script>

<template>
  <nav :class="['navbar', { 'scrolled': isScrolled }]">
    <div class="container nav-content">
      <a href="#" class="logo">
        <span class="text-gradient">Portfolio.</span>
      </a>
      <ul class="nav-links">
        <li><a href="#about">Giới Thiệu</a></li>
        <li><a href="#projects">Dự Án</a></li>
        <li><a href="#contact">Liên Hệ</a></li>
      </ul>
      <div class="nav-actions">
        <button class="theme-toggle" @click="toggleTheme" :aria-label="theme === 'dark' ? 'Chuyển sang nền sáng' : 'Chuyển sang nền tối'">
          <i v-if="theme === 'dark'" class="fa-solid fa-sun"></i>
          <i v-else class="fa-solid fa-moon"></i>
        </button>
        <a href="#contact" class="btn btn-outline nav-cta">Kết Nối Ngay</a>
      </div>
    </div>
  </nav>
</template>

<style scoped>
.navbar {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  z-index: 1000;
  padding: 1.5rem 0;
  transition: var(--transition);
}

.navbar.scrolled {
  background: var(--bg-dark);
  backdrop-filter: blur(12px);
  padding: 1rem 0;
  border-bottom: 1px solid var(--border-glass);
}

/* Semi-transparent background for scrolled navbar */
:root[data-theme="dark"] .navbar.scrolled {
  background: rgba(5, 5, 5, 0.85);
}
:root[data-theme="light"] .navbar.scrolled {
  background: rgba(248, 250, 252, 0.85);
}

.nav-content {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.logo {
  font-size: 1.5rem;
  font-weight: 800;
  text-decoration: none;
}

.nav-links {
  display: flex;
  gap: 2.5rem;
  list-style: none;
}

.nav-links a {
  color: var(--text-main);
  text-decoration: none;
  font-weight: 500;
  transition: var(--transition);
}

.nav-links a:hover {
  color: var(--accent);
}

.nav-actions {
  display: flex;
  align-items: center;
  gap: 1rem;
}

.theme-toggle {
  background: var(--bg-glass);
  border: 1px solid var(--border-glass);
  color: var(--text-main);
  width: 40px;
  height: 40px;
  border-radius: 50%;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: var(--transition);
  font-size: 1.1rem;
}

.theme-toggle:hover {
  background: var(--bg-glass-hover);
  color: var(--accent);
}

@media (max-width: 768px) {
  .nav-links { display: none; }
  .nav-cta { display: none; }
}
</style>
