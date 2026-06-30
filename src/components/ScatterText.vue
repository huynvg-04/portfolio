<script setup>
import { ref, onMounted, onUnmounted } from 'vue';
import gsap from 'gsap';

const props = defineProps({
  text: {
    type: String,
    required: true
  }
});

const container = ref(null);
const letters = ref([]);

let animationFrameId = null;
let mouse = { x: -9999, y: -9999 };
let isHovering = false;

const handleMouseMove = (e) => {
  mouse.x = e.clientX;
  mouse.y = e.clientY;

  if (!container.value) return;
  const rect = container.value.getBoundingClientRect();
  const padding = 200;
  if (
    mouse.x > rect.left - padding &&
    mouse.x < rect.right + padding &&
    mouse.y > rect.top - padding &&
    mouse.y < rect.bottom + padding
  ) {
    isHovering = true;
  } else if (isHovering) {
    handleMouseLeave();
  }
};

const handleMouseLeave = () => {
  if (!isHovering) return;
  isHovering = false;
  // Reset all letters
  if (letters.value) {
    letters.value.forEach((el) => {
      if (el) {
        gsap.to(el, {
          x: 0,
          y: 0,
          rotation: 0,
          scale: 1,
          duration: 3.5,
          ease: 'elastic.out(1, 0.2)',
          overwrite: 'auto'
        });
      }
    });
  }
};

const animate = () => {
  if (isHovering && letters.value) {
    const repelRadius = 250;
    const time = Date.now() * 0.002;
    
    letters.value.forEach((el, index) => {
      if (!el) return;
      const rect = el.getBoundingClientRect();
      const elCenterX = rect.left + rect.width / 2;
      const elCenterY = rect.top + rect.height / 2;
      
      const dx = elCenterX - mouse.x;
      const dy = elCenterY - mouse.y;
      const dist = Math.sqrt(dx * dx + dy * dy);
      
      if (dist < repelRadius) {
        const force = (repelRadius - dist) / repelRadius;
        
        // Base repel vector (pushes them outward)
        const targetX = (dx / dist) * force * 150; 
        const targetY = (dy / dist) * force * 150;
        
        // Chaotic space floating using sin/cos waves
        const floatX = Math.sin(time + index * 1.5) * 60 * force;
        const floatY = Math.cos(time * 0.8 + index * 2.0) * 60 * force;
        const floatRot = Math.sin(time * 1.2 + index) * 90 * force;
        
        gsap.to(el, {
          x: targetX + floatX,
          y: targetY + floatY,
          rotation: targetX * 0.2 + floatRot,
          scale: 1 + Math.sin(time + index) * 0.4 * force, // Adds a 3D depth illusion
          duration: 0.5,
          ease: 'power2.out',
          overwrite: 'auto'
        });
      } else {
        gsap.to(el, {
          x: 0,
          y: 0,
          rotation: 0,
          scale: 1,
          duration: 3.5,
          ease: 'elastic.out(1, 0.2)',
          overwrite: 'auto'
        });
      }
    });
  }
  
  animationFrameId = requestAnimationFrame(animate);
};

onMounted(() => {
  window.addEventListener('mousemove', handleMouseMove);
  animate();
});

onUnmounted(() => {
  window.removeEventListener('mousemove', handleMouseMove);
  if (animationFrameId) cancelAnimationFrame(animationFrameId);
});
</script>

<template>
  <span ref="container" class="scatter-text-container">
    <span v-for="(char, i) in text.split('')" :key="i" ref="letters" class="scatter-char"
      v-html="char === ' ' ? '&nbsp;' : char"></span>
  </span>
</template>

<style scoped>
.scatter-text-container {
  display: inline-block;
  cursor: default;
}

.scatter-char {
  display: inline-block;
  will-change: transform;
}

.scatter-char:hover {
  color: var(--accent);
  transition: color 0.2s;
}
</style>
