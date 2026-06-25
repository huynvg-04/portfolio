<script setup>
import { ref, onMounted, onUnmounted } from 'vue';

const roles = [
  'Fresh Full-Stack Developer',
  'PHP & Laravel Developer',
  'Vue.js Developer',
  'Fresh Graduate Web Developer',
];
const displayedRole = ref('');
const roleIndex     = ref(0);
const charIndex     = ref(0);
const isDeleting    = ref(false);
let   typingTimer   = null;

const type = () => {
  const current = roles[roleIndex.value];
  if (!isDeleting.value) {
    displayedRole.value = current.slice(0, charIndex.value + 1);
    charIndex.value++;
    if (charIndex.value === current.length) {
      isDeleting.value = true;
      typingTimer = setTimeout(type, 1800);
      return;
    }
  } else {
    displayedRole.value = current.slice(0, charIndex.value - 1);
    charIndex.value--;
    if (charIndex.value === 0) {
      isDeleting.value = false;
      roleIndex.value  = (roleIndex.value + 1) % roles.length;
    }
  }
  typingTimer = setTimeout(type, isDeleting.value ? 60 : 90);
};

// ── Stats counter ──
const stats = [
  { label: 'Kinh nghiệm thực tế', value: 1, suffix: ' năm+' },
  { label: 'Dự án', value: 3 },
  { label: 'Công nghệ nắm vững', value: 12, suffix: '+' },
];

const counted    = ref([0, 0, 0]);
const countDone  = ref(false);

const runCounter = () => {
  if (countDone.value) return;
  countDone.value = true;
  stats.forEach((s, i) => {
    let start = 0;
    const step = Math.ceil(s.value / 40);
    const tick = setInterval(() => {
      start = Math.min(start + step, s.value);
      counted.value[i] = start;
      if (start >= s.value) clearInterval(tick);
    }, 40);
  });
};

// ── Canvas particles ──
const canvasRef = ref(null);
let   animFrame = null;

const initParticles = (canvas) => {
  const ctx     = canvas.getContext('2d');
  let   W = canvas.width  = canvas.offsetWidth;
  let   H = canvas.height = canvas.offsetHeight;

  const particles = Array.from({ length: 60 }, () => ({
    x:  Math.random() * W,
    y:  Math.random() * H,
    r:  Math.random() * 1.5 + 0.5,
    vx: (Math.random() - 0.5) * 0.4,
    vy: (Math.random() - 0.5) * 0.4,
    opacity: Math.random() * 0.5 + 0.1,
  }));

  const draw = () => {
    ctx.clearRect(0, 0, W, H);
    particles.forEach(p => {
      p.x += p.vx;
      p.y += p.vy;
      if (p.x < 0 || p.x > W) p.vx *= -1;
      if (p.y < 0 || p.y > H) p.vy *= -1;
      ctx.beginPath();
      ctx.arc(p.x, p.y, p.r, 0, Math.PI * 2);
      ctx.fillStyle = `rgba(56, 189, 248, ${p.opacity})`;
      ctx.fill();
    });
    // Draw lines
    for (let i = 0; i < particles.length; i++) {
      for (let j = i + 1; j < particles.length; j++) {
        const dx = particles[i].x - particles[j].x;
        const dy = particles[i].y - particles[j].y;
        const dist = Math.sqrt(dx * dx + dy * dy);
        if (dist < 130) {
          ctx.beginPath();
          ctx.moveTo(particles[i].x, particles[i].y);
          ctx.lineTo(particles[j].x, particles[j].y);
          ctx.strokeStyle = `rgba(56, 189, 248, ${0.12 * (1 - dist / 130)})`;
          ctx.lineWidth   = 0.7;
          ctx.stroke();
        }
      }
    }
    animFrame = requestAnimationFrame(draw);
  };

  draw();

  const resize = () => {
    W = canvas.width  = canvas.offsetWidth;
    H = canvas.height = canvas.offsetHeight;
  };
  window.addEventListener('resize', resize);
};

onMounted(() => {
  typingTimer = setTimeout(type, 600);
  if (canvasRef.value) initParticles(canvasRef.value);

  // Trigger counter when hero is in view
  const heroEl = document.getElementById('hero');
  if (heroEl) {
    const obs = new IntersectionObserver((entries) => {
      if (entries[0].isIntersecting) { runCounter(); obs.disconnect(); }
    }, { threshold: 0.4 });
    obs.observe(heroEl);
  }
});

onUnmounted(() => {
  clearTimeout(typingTimer);
  if (animFrame) cancelAnimationFrame(animFrame);
});
</script>

<template>
  <section id="hero" class="hero-section">
    <!-- Particle Canvas -->
    <canvas ref="canvasRef" class="hero-canvas"></canvas>

    <div class="container hero-content">
      <!-- Left: Text -->
      <div class="hero-text">
        <div class="badge fade-blur visible" data-delay="0">
          <span class="dot"></span>
          Sẵn sàng nhận dự án mới
        </div>

        <h1 class="hero-title fade-up visible" data-delay="100">
          Xin chào, tôi là<br/>
          <span class="text-gradient typing-wrapper">
            {{ displayedRole }}<span class="cursor">|</span>
          </span>
        </h1>

        <p class="hero-subtitle fade-up visible" data-delay="200">
          Là một lập trình viên mới tốt nghiệp, tôi tập trung xây dựng các trải nghiệm web tối ưu, nhanh chóng và thân thiện — kết hợp công nghệ hiện đại với tư duy thiết kế sáng tạo.
        </p>

        <!-- Actions -->
        <div class="hero-actions fade-up visible" data-delay="300">
          <a href="#projects" class="btn btn-primary" id="hero-cta-projects">
            Xem Sản Phẩm <i class="fa-solid fa-arrow-right"></i>
          </a>
          <a href="#contact" class="btn btn-outline" id="hero-cta-contact">
            Liên Hệ Ngay
          </a>
        </div>

        <!-- Social Icons -->
        <div class="social-links fade-up visible" data-delay="400">
          <a href="https://github.com/huynvg-04/" class="social-icon" target="_blank" rel="noopener" aria-label="GitHub">
            <i class="fa-brands fa-github"></i>
          </a>
          <a href="https://www.linkedin.com/in/ngo-van-gia-huy04" class="social-icon" target="_blank" rel="noopener" aria-label="LinkedIn">
            <i class="fa-brands fa-linkedin"></i>
          </a>
          <a href="https://www.facebook.com/em.an.com.chua.UwU" class="social-icon" target="_blank" rel="noopener" aria-label="Facebook">
            <i class="fa-brands fa-facebook"></i>
          </a>
        </div>
      </div>

      <!-- Right: Visual -->
      <div class="hero-visual fade-right visible" data-delay="200">
        <!-- Avatar ring -->
        <div class="avatar-ring">
          <div class="avatar-inner">
            <i class="fa-solid fa-code avatar-icon"></i>
          </div>
          <svg class="ring-svg" viewBox="0 0 200 200">
            <circle cx="100" cy="100" r="90" class="ring-track"/>
            <circle cx="100" cy="100" r="90" class="ring-progress"/>
          </svg>
        </div>

        <!-- Code card -->
        <div class="glass-card code-card animate-float">
          <div class="code-header">
            <div class="code-dots">
              <span class="dot-red"></span>
              <span class="dot-yellow"></span>
              <span class="dot-green"></span>
            </div>
            <span class="code-filename">developer.js</span>
          </div>
          <div class="code-body">
            <div class="code-line"><span class="ln">1</span> <span class="keyword">const</span> <span class="variable">developer</span> = {</div>
            <div class="code-line"><span class="ln">2</span>   <span class="property">name</span>: <span class="string">'Gia Huy'</span>,</div>
            <div class="code-line"><span class="ln">3</span>   <span class="property">skills</span>: [<span class="string">'PHP'</span>, <span class="string">'Laravel'</span>, <span class="string">'Vue'</span>],</div>
            <div class="code-line"><span class="ln">4</span>   <span class="property">passion</span>: <span class="string">'Giao diện tinh tế'</span>,</div>
            <div class="code-line"><span class="ln">5</span>   <span class="property">available</span>: <span class="keyword">true</span></div>
            <div class="code-line"><span class="ln">6</span> };</div>
          </div>
        </div>

        <!-- Stats -->
        <div class="stats-row">
          <div class="stat-item" v-for="(s, i) in stats" :key="i">
            <span class="stat-val">{{ counted[i] }}{{ s.suffix }}</span>
            <span class="stat-label">{{ s.label }}</span>
          </div>
        </div>
      </div>
    </div>

    <!-- Scroll indicator -->
    <a href="#about" class="scroll-indicator" aria-label="Cuộn xuống">
      <div class="scroll-mouse">
        <div class="scroll-dot"></div>
      </div>
    </a>
  </section>
</template>

<style scoped>
/* ── Section ── */
.hero-section {
  min-height: 100vh;
  display: flex;
  align-items: center;
  padding-top: 90px;
  padding-bottom: 4rem;
  position: relative;
  overflow: hidden;
}

/* ── Canvas ── */
.hero-canvas {
  position: absolute;
  inset: 0;
  width: 100%;
  height: 100%;
  pointer-events: none;
  z-index: 0;
}

/* ── Grid ── */
.hero-content {
  display: grid;
  grid-template-columns: 1.1fr 0.9fr;
  gap: 5rem;
  align-items: center;
  position: relative;
  z-index: 1;
}

/* ── Badge ── */
.badge {
  display: inline-flex;
  align-items: center;
  gap: 0.6rem;
  padding: 0.45rem 1.1rem;
  background: var(--bg-glass);
  border: 1px solid var(--border-glass);
  border-radius: 50px;
  font-size: 0.85rem;
  font-weight: 500;
  margin-bottom: 2rem;
  color: var(--text-muted);
  backdrop-filter: blur(8px);
}

.dot {
  width: 8px;
  height: 8px;
  border-radius: 50%;
  background: var(--neon-green);
  box-shadow: 0 0 8px var(--neon-green);
  animation: dot-pulse 2s ease-in-out infinite;
  flex-shrink: 0;
}

/* ── Title ── */
.hero-title {
  font-size: clamp(1.25rem, 5vw, 3.75rem);
  margin-bottom: 1.5rem;
  letter-spacing: -0.03em;
  line-height: 1.1;
}

.typing-wrapper {
  display: inline-block;
  min-width: 2px;
}

.cursor {
  animation: blink 1s step-end infinite;
  color: var(--accent);
  font-weight: 300;
}

.hero-subtitle {
  font-size: clamp(0.95rem, 2.5vw, 1.15rem);
  color: var(--text-muted);
  max-width: 520px;
  margin-bottom: 2.5rem;
  line-height: 1.75;
}

.hero-subtitle strong {
  color: var(--text-main);
  font-weight: 600;
}

/* ── Actions ── */
.hero-actions {
  display: flex;
  align-items: center;
  gap: 1rem;
  margin-bottom: 2.5rem;
  flex-wrap: wrap;
}

/* ── Social ── */
.social-links {
  display: flex;
  gap: 0.85rem;
}

.social-icon {
  width: 44px;
  height: 44px;
  border-radius: 50%;
  background: var(--bg-glass);
  display: flex;
  align-items: center;
  justify-content: center;
  color: var(--text-muted);
  text-decoration: none;
  font-size: 1.15rem;
  transition: var(--transition);
  border: 1px solid var(--border-glass);
}

.social-icon:hover {
  background: var(--accent-gradient);
  color: #fff;
  transform: translateY(-4px) scale(1.05);
  border-color: transparent;
  box-shadow: 0 8px 20px var(--shadow-glow);
}

/* ── Visual Column ── */
.hero-visual {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 2rem;
  position: relative;
}

/* ── Avatar Ring ── */
.avatar-ring {
  position: relative;
  width: 160px;
  height: 160px;
  flex-shrink: 0;
}

.avatar-inner {
  position: absolute;
  inset: 12px;
  border-radius: 50%;
  background: var(--card-gradient);
  border: 2px solid var(--border-glass);
  display: flex;
  align-items: center;
  justify-content: center;
  backdrop-filter: blur(10px);
}

.avatar-icon {
  font-size: 3.5rem;
  background: var(--accent-gradient);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

.ring-svg {
  position: absolute;
  inset: 0;
  width: 100%;
  height: 100%;
  animation: spin-slow 8s linear infinite;
}

.ring-track {
  fill: none;
  stroke: var(--border-glass);
  stroke-width: 2;
}

.ring-progress {
  fill: none;
  stroke: url(#ringGrad);
  stroke-width: 2;
  stroke-dasharray: 565;
  stroke-dashoffset: 200;
  stroke-linecap: round;
}

/* ── Code Card ── */
.code-card {
  width: 100%;
  padding: 0;
  overflow: hidden;
}

.code-header {
  display: flex;
  align-items: center;
  gap: 0.75rem;
  padding: 0.75rem 1.25rem;
  border-bottom: 1px solid var(--border-glass);
  background: rgba(0,0,0,0.15);
}

.code-dots {
  display: flex;
  gap: 6px;
}

.dot-red, .dot-yellow, .dot-green {
  width: 12px;
  height: 12px;
  border-radius: 50%;
}
.dot-red    { background: #ff5f57; }
.dot-yellow { background: #febc2e; }
.dot-green  { background: #28c840; }

.code-filename {
  font-family: var(--font-code);
  font-size: 0.8rem;
  color: var(--text-muted);
  margin-left: auto;
}

.code-body {
  padding: 1.25rem 1.5rem;
  font-family: var(--font-code);
  font-size: 0.9rem;
  line-height: 1.9;
}

.code-line {
  display: flex;
  gap: 1rem;
}

.ln {
  color: var(--text-dim);
  user-select: none;
  min-width: 1ch;
  text-align: right;
}

.keyword  { color: #f472b6; }
.variable { color: #38bdf8; }
.property { color: #a78bfa; }
.string   { color: #4ade80; }

:root[data-theme="light"] .keyword  { color: #db2777; }
:root[data-theme="light"] .variable { color: #0284c7; }
:root[data-theme="light"] .property { color: #7c3aed; }
:root[data-theme="light"] .string   { color: #16a34a; }

/* ── Stats Row ── */
.stats-row {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 1rem;
  width: 100%;
}

.stat-item {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0.25rem;
  padding: 1rem 0.5rem;
  background: var(--bg-glass);
  border: 1px solid var(--border-glass);
  border-radius: var(--radius-sm);
  backdrop-filter: blur(8px);
  transition: var(--transition);
}

.stat-item:hover {
  border-color: var(--accent);
  background: var(--accent-dim);
}

.stat-val {
  font-size: 1.6rem;
  font-weight: 800;
  background: var(--accent-gradient);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
  line-height: 1;
}

.stat-label {
  font-size: 0.72rem;
  color: var(--text-muted);
  text-align: center;
  font-weight: 500;
}

/* ── Scroll Indicator ── */
.scroll-indicator {
  position: absolute;
  bottom: 2rem;
  left: 50%;
  transform: translateX(-50%);
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0.5rem;
  text-decoration: none;
  animation: float 3s ease-in-out infinite;
}

.scroll-mouse {
  width: 24px;
  height: 40px;
  border: 2px solid var(--border-glass-hover);
  border-radius: 12px;
  display: flex;
  justify-content: center;
  padding-top: 6px;
}

.scroll-dot {
  width: 4px;
  height: 8px;
  background: var(--accent);
  border-radius: 2px;
  animation: float 1.5s ease-in-out infinite;
}

/* ── Responsive ── */
/* Tablet ≤ 992px */
@media (max-width: 992px) {
  .hero-content {
    grid-template-columns: 1fr;
    text-align: center;
    gap: 3rem;
  }
  .hero-subtitle   { margin: 0 auto 2.5rem; }
  .hero-actions    { justify-content: center; }
  .social-links    { justify-content: center; }
  .hero-visual     { order: -1; }
  .stats-row       { max-width: 420px; margin: 0 auto; }
  .scroll-indicator { display: none; }
}

/* Mobile ≤ 768px */
@media (max-width: 768px) {
  .hero-section {
    padding-top: 80px;
    padding-bottom: 3rem;
    min-height: 100svh;
  }
  .hero-content { gap: 2.5rem; }
  .badge { font-size: 0.78rem; padding: 0.4rem 0.9rem; }
  .avatar-ring { width: 130px; height: 130px; }
  .avatar-icon { font-size: 2.8rem; }
  .code-body { font-size: 0.82rem; padding: 1rem 1.25rem; }
  .stat-val { font-size: 1.4rem; }
  .stat-label { font-size: 0.68rem; }
  .hero-actions { gap: 0.75rem; }
}

/* Small Mobile ≤ 480px */
@media (max-width: 480px) {
  .hero-section { padding-top: 75px; }
  .hero-actions { flex-direction: column; width: 100%; }
  .hero-actions .btn { width: 100%; justify-content: center; }
  .badge { font-size: 0.75rem; padding: 0.35rem 0.85rem; }
  .avatar-ring { width: 110px; height: 110px; }
  .avatar-icon { font-size: 2.4rem; }
  .stats-row { grid-template-columns: repeat(3, 1fr); gap: 0.6rem; max-width: 100%; }
  .stat-item { padding: 0.75rem 0.25rem; }
  .stat-val { font-size: 1.25rem; }
  .stat-label { font-size: 0.62rem; }
  .code-body { font-size: 0.78rem; padding: 0.85rem 1rem; line-height: 1.7; }
  .social-links { gap: 0.6rem; }
  .social-icon { width: 40px; height: 40px; font-size: 1rem; }
}

/* Very Small ≤ 360px */
@media (max-width: 360px) {
  .hero-content { gap: 2rem; }
  .avatar-ring { width: 100px; height: 100px; }
  .avatar-icon { font-size: 2rem; }
  .stats-row { gap: 0.5rem; }
  .stat-item { padding: 0.6rem 0.15rem; }
  .stat-val { font-size: 1.1rem; }
  .stat-label { font-size: 0.58rem; }
  .code-body { padding: 0.75rem 0.875rem; font-size: 0.72rem; }
}
</style>
