<script setup>
import Navbar from './components/Navbar.vue';
import Hero from './components/Hero.vue';
import About from './components/About.vue';
import Projects from './components/Projects.vue';
import Contact from './components/Contact.vue';
</script>

<template>
  <div class="app-wrapper">
    <Navbar />
    <main>
      <Hero />
      <About />
      <Projects />
      <Contact />
    </main>
    <footer class="footer">
      <p>&copy; 2026 Hồ Sơ Cá Nhân Vue. Thiết kế với <i class="fa-solid fa-heart" style="color: #e94057;"></i></p>
    </footer>
  </div>
</template>

<style scoped>
.footer {
  text-align: center;
  padding: 2rem 0;
  border-top: 1px solid var(--border-glass);
  color: var(--text-muted);
  background: var(--bg-dark);
}
</style>
