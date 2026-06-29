<script setup>
import { ref, onMounted, onUnmounted } from 'vue';

const isScrolled = ref(false);
const menuOpen = ref(false);
const scrollProgress = ref(0);
const activeSection = ref('hero');

const sections = ['hero', 'about', 'projects', 'contact'];



const toggleMenu = () => { menuOpen.value = !menuOpen.value; };
const closeMenu = () => { menuOpen.value = false; };

const handleScroll = () => {
  const scrollY = window.scrollY;
  const docHeight = document.documentElement.scrollHeight - window.innerHeight;
  isScrolled.value = scrollY > 60;
  scrollProgress.value = docHeight > 0 ? (scrollY / docHeight) * 100 : 0;

  for (const id of sections) {
    const el = document.getElementById(id);
    if (!el) continue;
    const { top } = el.getBoundingClientRect();
    if (top <= 120) activeSection.value = id;
  }
};

onMounted(() => {
  window.addEventListener('scroll', handleScroll, { passive: true });


});

onUnmounted(() => window.removeEventListener('scroll', handleScroll));
</script>

<template>
  <div class="scroll-progress" :style="{ width: scrollProgress + '%' }"></div>

  <nav :class="['navbar', { 'scrolled': isScrolled }]">
    <div class="nav-content">
      <a href="#hero" class="logo" @click="closeMenu">
        <span class="logo-bracket">&lt;</span>
        <span class="text-gradient">huynvg-04</span>
        <span class="logo-bracket">/&gt;</span>
      </a>

      <div class="nav-actions">

        <a href="mailto:ngovangiahuy04@gmail.com" class="nav-email-link" @click="closeMenu">ngovangiahuy04@gmail.com</a>
        <button class="hamburger" @click="toggleMenu" :aria-expanded="menuOpen" aria-label="Menu">
          <span :class="{ open: menuOpen }"></span>
          <span :class="{ open: menuOpen }"></span>
          <span :class="{ open: menuOpen }"></span>
        </button>
      </div>

      <ul class="nav-links">
        <li v-for="s in [['#about', 'About'], ['#projects', 'Projects'], ['#contact', 'Contact']]" :key="s[0]">
          <a :href="s[0]" :class="{ active: activeSection === s[0].slice(1) }">
            {{ s[1] }}
          </a>
        </li>
      </ul>
    </div>
  </nav>

  <div :class="['mobile-drawer', { open: menuOpen }]" @click.self="closeMenu">
    <div class="drawer-content">
      <ul>
        <li><a href="#about" @click="closeMenu">About</a></li>
        <li><a href="#projects" @click="closeMenu">Projects</a></li>
        <li><a href="#contact" @click="closeMenu">Contact</a></li>
      </ul>
      <a href="#contact" class="btn btn-primary drawer-cta" @click="closeMenu">Let's Connect</a>
    </div>
  </div>
</template>

<style scoped>
.scroll-progress {
  position: fixed;
  top: 0;
  left: 0;
  height: 3px;
  background: var(--accent-gradient);
  z-index: 9999;
  transition: width 0.1s linear;
  border-radius: 0 2px 2px 0;
  box-shadow: 0 0 10px var(--shadow-glow);
}

.navbar {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  z-index: 1000;
  padding: 1.4rem 0;
  border-bottom: 1px solid transparent;
  background: rgba(5, 4, 5, 0.5);
  backdrop-filter: blur(8px);
  -webkit-backdrop-filter: blur(8px);
  -webkit-mask-image: linear-gradient(to bottom, black 50%, transparent 100%);
  mask-image: linear-gradient(to bottom, black 50%, transparent 100%);
  transition:
    border-color 0.3s cubic-bezier(0.4, 0, 0.2, 1),
    background 0.3s cubic-bezier(0.4, 0, 0.2, 1),
    backdrop-filter 0.3s cubic-bezier(0.4, 0, 0.2, 1);
}



.nav-content {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin: 0;
  width: 100%;
  padding: 0 3rem;
}

.logo {
  font-size: 1.4rem;
  font-weight: 800;
  text-decoration: none;
  display: flex;
  align-items: center;
  gap: 2px;
  letter-spacing: -0.02em;
}

.logo-bracket {
  color: var(--text-muted);
  font-weight: 300;
  font-family: var(--font-code);
}

.nav-links {
  display: flex;
  gap: 2.5rem;
  list-style: none;
}

.nav-links a {
  color: var(--text-muted);
  text-decoration: none;
  font-weight: 500;
  font-size: 0.95rem;
  transition: var(--transition);
  position: relative;
  padding-bottom: 4px;
}

.nav-links a::after {
  content: '';
  position: absolute;
  bottom: 0;
  left: 0;
  width: 0;
  height: 2px;
  background: var(--accent-gradient);
  border-radius: 2px;
  transition: width 0.3s ease;
}

.nav-links a:hover,
.nav-links a.active {
  color: var(--text-main);
}

.nav-links a:hover::after,
.nav-links a.active::after {
  width: 100%;
}

.nav-actions {
  display: flex;
  align-items: center;
  gap: 0.75rem;
}

.nav-email-link {
  color: var(--text-muted);
  text-decoration: none;
  font-weight: 500;
  font-size: 0.95rem;
  transition: color 0.3s ease;
}

.nav-email-link:hover {
  color: var(--accent);
}



.hamburger {
  display: none;
  flex-direction: column;
  gap: 5px;
  background: none;
  border: none;
  cursor: pointer;
  padding: 6px;
}

.hamburger span {
  display: block;
  width: 24px;
  height: 2px;
  background: var(--text-main);
  border-radius: 2px;
  transition: var(--transition);
  transform-origin: center;
}

.hamburger span.open:nth-child(1) {
  transform: translateY(7px) rotate(45deg);
}

.hamburger span.open:nth-child(2) {
  opacity: 0;
  transform: scaleX(0);
}

.hamburger span.open:nth-child(3) {
  transform: translateY(-7px) rotate(-45deg);
}

.mobile-drawer {
  display: none;
  position: fixed;
  inset: 0;
  z-index: 999;
  background: rgba(0, 0, 0, 0.5);
  backdrop-filter: blur(4px);
  opacity: 0;
  pointer-events: none;
  transition: opacity 0.3s ease;
}

.mobile-drawer.open {
  opacity: 1;
  pointer-events: all;
}

.drawer-content {
  position: absolute;
  top: 0;
  right: 0;
  width: min(80vw, 320px);
  height: 100%;
  background: var(--bg-surface);
  border-left: 1px solid var(--border-glass);
  padding: 6rem 2.5rem 2.5rem;
  display: flex;
  flex-direction: column;
  gap: 2rem;
  transform: translateX(100%);
  transition: transform 0.35s cubic-bezier(0.4, 0, 0.2, 1);
}

.mobile-drawer.open .drawer-content {
  transform: translateX(0);
}

.drawer-content ul {
  list-style: none;
  display: flex;
  flex-direction: column;
  gap: 1rem;
}

.drawer-content ul a {
  font-size: 1.35rem;
  font-weight: 600;
  color: var(--text-main);
  text-decoration: none;
  transition: color 0.2s;
  display: block;
  padding: 0.5rem 0;
  border-bottom: 1px solid var(--border-glass);
}

.drawer-content ul a:hover {
  color: var(--accent);
}

.drawer-cta {
  justify-content: center;
  width: 100%;
}

@media (max-width: 768px) {
  .nav-content {
    padding: 0 1.5rem;
  }

  .nav-links {
    display: none;
  }

  .nav-cta {
    display: none;
  }

  .hamburger {
    display: flex;
  }

  .mobile-drawer {
    display: block;
  }

  .navbar {
    padding: 1rem 0;
  }

  .navbar.scrolled {
    padding: 0.75rem 0;
  }
}

@media (max-width: 480px) {
  .logo {
    font-size: 1.2rem;
  }

  .hamburger {
    padding: 4px;
  }
}

@media (max-width: 360px) {
  .logo {
    font-size: 1.1rem;
  }

  .drawer-content {
    padding: 5rem 1.75rem 2rem;
  }

  .drawer-content ul a {
    font-size: 1.2rem;
  }
}
</style>
