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
  { icon: 'fa-solid fa-wind', color: '#38bdf8', name: 'Tailwind CSS' },
  { icon: 'fa-solid fa-database', color: '#fb923c', name: 'SQL' },
  { icon: 'fa-brands fa-git-alt', color: '#f05032', name: 'Git' },
  { icon: 'fa-brands fa-figma', color: '#a259ff', name: 'Figma' },
];

onMounted(() => {
  const observer = new IntersectionObserver((entries) => {
    entries.forEach(entry => {
      if (entry.isIntersecting) {
        entry.target.classList.add('visible');
      }
    });
  }, { threshold: 0.1 });

  document.querySelectorAll('.animate-on-scroll').forEach(el => observer.observe(el));
});
</script>

<template>
  <section id="about" class="about-section">

  </section>
</template>

<style scoped></style>
