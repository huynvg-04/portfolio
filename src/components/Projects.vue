<script setup>
import { ref, computed, onMounted, onUnmounted, watch, nextTick } from 'vue';

const activeFilter = ref('All');

const allProjects = [
  {
    num: '01',
    title: 'E-Commerce Platform',
    subtitle: 'VNPAY & MoMo Payment — Real-time Cart — Admin Dashboard',
    category: 'Fullstack',
    tech: ['Laravel', 'MySQL', 'HTML', 'CSS', 'JavaScript'],
    color: '#38bdf8',
    github: 'https://github.com/huynvg-04/greenvibes',
    image: '/greenvibes.png',
  },
  {
    num: '02',
    title: 'Movie Website',
    subtitle: 'Online Movie Streaming — Search & Genre Classification',
    category: 'Fullstack',
    tech: ['Laravel', 'MySQL', 'HTML', 'CSS'],
    color: '#a855f7',
    github: 'https://github.com/huynvg-04/moviewebsite',
    image: '/moviewebsite.png',
  },
];

const skeletons = [
  { num: '03', color: '#6366f1' },
  { num: '04', color: '#f59e0b' },
];

const filters = computed(() => [
  'All',
  ...new Set(allProjects.map(p => p.category)),
]);

const visibleItems = computed(() => {
  const real =
    activeFilter.value === 'All'
      ? allProjects
      : allProjects.filter(p => p.category === activeFilter.value);
  return [
    ...real.map(p => ({ ...p, isSkeleton: false })),
    ...skeletons.map(s => ({ ...s, isSkeleton: true })),
  ];
});

/* ── Sticky horizontal scroll ── */
const sectionRef = ref(null);
const trackRef = ref(null);
const scrollProgress = ref(0);

const activeIndex = computed(() =>
  Math.min(
    Math.round(scrollProgress.value * (visibleItems.value.length - 1)),
    visibleItems.value.length - 1,
  ),
);

const updateTranslate = () => {
  const section = sectionRef.value;
  const track = trackRef.value;
  if (!section || !track) return;

  const sectionTop = section.offsetTop;
  const scrolled = window.scrollY - sectionTop;
  const maxScroll = section.offsetHeight - window.innerHeight;
  if (maxScroll <= 0) return;

  const progress = Math.min(Math.max(scrolled / maxScroll, 0), 1);
  scrollProgress.value = progress;

  const totalTrack = track.scrollWidth - window.innerWidth;
  track.style.transform = `translateX(${-progress * totalTrack}px)`;
};

onMounted(() => {
  window.addEventListener('scroll', updateTranslate, { passive: true });
  updateTranslate();
});
onUnmounted(() => window.removeEventListener('scroll', updateTranslate));
watch(visibleItems, () => nextTick(updateTranslate));
</script>

<template>
  <section id="projects" class="projects-section" ref="sectionRef"
    :style="{ height: visibleItems.length * 100 + 'vh' }">
    <div class="projects-sticky">

      <!-- ── Header ─────────────────────────────── -->
      <div class="proj-header">
        <h2 class="proj-heading">My <span class="text-gradient">Projects</span></h2>
        <div class="filter-tabs">
          <button v-for="f in filters" :key="f" :class="['filter-btn', { active: activeFilter === f }]"
            @click="activeFilter = f">{{ f }}</button>
        </div>
      </div>

      <!-- ── Horizontal track ────────────────────── -->
      <div class="proj-track" ref="trackRef">
        <article v-for="(item, index) in visibleItems" :key="item.num" class="proj-slide"
          :style="{ '--c': item.color }">
          <!-- Watermark number -->
          <span class="wm-num" aria-hidden="true">{{ item.num }}</span>

          <!-- Left — Info -->
          <div class="slide-info">
            <div class="slide-top">
              <span class="slide-idx">{{ item.num }}</span>
              <div class="slide-meta">
                <template v-if="!item.isSkeleton">
                  <h3 class="slide-name">{{ item.title }}</h3>
                  <p class="slide-sub">{{ item.subtitle }}</p>
                </template>
                <template v-else>
                  <div class="skel skel-h"></div>
                  <div class="skel skel-p"></div>
                </template>
              </div>
            </div>

            <div class="slide-divider"></div>

            <template v-if="!item.isSkeleton">
              <div>
                <p class="tech-label">Technologies Used</p>
                <p class="tech-text">{{ item.tech.join(', ') }}</p>
              </div>
              <div class="slide-actions">
                <a :href="item.github" target="_blank" rel="noopener" class="slide-btn">
                  GitHub ↗
                </a>
              </div>
            </template>
            <template v-else>
              <div>
                <div class="skel skel-label"></div>
                <div class="skel skel-tech"></div>
              </div>
              <span class="coming-badge">Coming Soon</span>
            </template>
          </div>

          <!-- Right — Preview -->
          <div class="slide-preview">
            <template v-if="!item.isSkeleton">
              <img v-if="item.image" :src="item.image" :alt="item.title" class="project-image" />
              <div v-else class="mockup">
                <div class="mockup-bar">
                  <span class="mdot" v-for="i in 3" :key="i"></span>
                  <div class="murl"></div>
                </div>
                <div class="mockup-body">
                  <div class="m-sidebar">
                    <div class="ms-item" v-for="i in 5" :key="i"></div>
                  </div>
                  <div class="m-main">
                    <div class="m-line" style="width:70%"></div>
                    <div class="m-line" style="width:45%"></div>
                    <div class="m-cards">
                      <div class="m-card" v-for="i in 4" :key="i"></div>
                    </div>
                    <div class="m-line" style="width:60%"></div>
                  </div>
                </div>
              </div>
            </template>
            <template v-else>
              <div class="skel-preview-inner">
                <div class="skel skel-mbar"></div>
                <div class="skel-mbody">
                  <div class="skel skel-msidebar"></div>
                  <div class="skel-mmain">
                    <div class="skel skel-mline"></div>
                    <div class="skel skel-mline short"></div>
                    <div class="skel skel-mblock"></div>
                    <div class="skel skel-mline" style="width:60%"></div>
                  </div>
                </div>
              </div>
            </template>
          </div>

        </article>
      </div>

      <!-- ── Footer — dots + hint ───────────────── -->
      <div class="proj-footer">
        <div class="proj-dots">
          <span v-for="(item, i) in visibleItems" :key="i" class="proj-dot"
            :class="{ active: activeIndex === i }"></span>
        </div>
        <transition name="fade-hint">
          <span v-if="activeIndex < visibleItems.length - 1" class="scroll-hint" key="more">
            Scroll to see more →
          </span>
          <span v-else class="scroll-hint done" key="done">
            Scroll to continue ↓
          </span>
        </transition>
      </div>

    </div>
  </section>
</template>

<style scoped>
/* ─── Section / Sticky wrapper ─────────────────────────────── */
.projects-section {
  position: relative;
}

.projects-sticky {
  position: sticky;
  top: 0;
  height: 100vh;
  overflow: hidden;
  display: flex;
  flex-direction: column;
  background: var(--bg-dark);
}

.proj-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 6rem 5vw 1.25rem;
  border-bottom: 0.5px solid rgba(255, 255, 255, 0.352);
  flex-shrink: 0;
}

.proj-heading {
  font-size: clamp(2.24rem, 4.9vw, 3.36rem);
  font-weight: 500;
  line-height: 1;
}

.filter-tabs {
  display: flex;
  gap: 0.5rem;
}

.filter-btn {
  padding: 0.45rem 1.1rem;
  border-radius: 50px;
  border: 1px solid var(--border-glass);
  background: var(--bg-glass);
  color: var(--text-muted);
  font-family: var(--font-main);
  font-weight: 500;
  font-size: 0.85rem;
  cursor: pointer;
  transition: var(--transition);
  pointer-events: auto;
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

/* ─── Horizontal track ─────────────────────────────────────── */
.proj-track {
  display: flex;
  flex: 1;
  min-height: 0;
  will-change: transform;
  transition: transform 0.1s cubic-bezier(0.21, 0.85, 0.35, 1);
}

.proj-slide {
  min-width: 100vw;
  width: 100vw;
  height: 100%;
  display: grid;
  grid-template-columns: 1fr 1fr;
  align-items: center;
  padding: 2rem 6vw;
  gap: 4vw;
  box-sizing: border-box;
  position: relative;
  overflow: hidden;
  border-right: 0.5px solid rgba(255, 255, 255, 0.352);
}

.proj-slide:last-child {
  border-right: none;
}

/* Watermark giant number */
.wm-num {
  position: absolute;
  bottom: -0.12em;
  left: 4vw;
  font-size: clamp(7rem, 22vw, 20rem);
  font-weight: 900;
  line-height: 1;
  color: transparent;
  -webkit-text-stroke: 1px rgba(255, 255, 255, 0.035);
  pointer-events: none;
  user-select: none;
  z-index: 0;
}

/* ─── Slide Info (left) ─────────────────────────────────────── */
.slide-info {
  display: flex;
  flex-direction: column;
  gap: 1.75rem;
  position: relative;
  z-index: 1;
}

.slide-top {
  display: flex;
  align-items: flex-start;
  gap: 1.5rem;
}

.slide-idx {
  font-size: clamp(2.5rem, 5vw, 4rem);
  font-weight: 900;
  line-height: 1;
  color: var(--accent);
  flex-shrink: 0;
  letter-spacing: -0.02em;
}

.slide-meta {
  flex: 1;
  padding-top: 0.35rem;
}

.slide-name {
  font-size: clamp(1.2rem, 2.2vw, 1.75rem);
  font-weight: 700;
  line-height: 1.2;
  margin-bottom: 0.5rem;
  color: var(--text-main);
}

.slide-sub {
  font-size: 0.85rem;
  color: var(--text-muted);
  line-height: 1.6;
}

.slide-divider {
  height: 1px;
  background: linear-gradient(90deg, var(--accent) 0%, transparent 80%);
  opacity: 0.35;
}

.tech-label {
  font-size: 0.72rem;
  text-transform: uppercase;
  letter-spacing: 0.12em;
  color: var(--text-muted);
  margin-bottom: 0.5rem;
}

.tech-text {
  font-size: clamp(0.95rem, 1.5vw, 1.1rem);
  color: var(--text-main);
  line-height: 1.65;
}

.slide-actions {
  display: flex;
  gap: 0.75rem;
}

.slide-btn {
  display: inline-flex;
  align-items: center;
  padding: 0.65rem 1.6rem;
  border: 1px solid var(--border-glass-hover);
  border-radius: 50px;
  color: var(--text-main);
  text-decoration: none;
  font-size: 0.9rem;
  font-weight: 500;
  font-family: var(--font-main);
  background: var(--bg-glass);
  transition: var(--transition);
  cursor: pointer;
  pointer-events: auto;
}

.slide-btn:hover {
  border-color: var(--accent);
  color: var(--accent);
  background: var(--bg-glass-hover);
}

.coming-badge {
  display: inline-flex;
  align-items: center;
  align-self: flex-start;
  padding: 0.5rem 1.25rem;
  border: 1px dashed var(--border-glass-hover);
  border-radius: 50px;
  color: var(--text-muted);
  font-size: 0.83rem;
  animation: cpulse 2s ease-in-out infinite;
}

@keyframes cpulse {

  0%,
  100% {
    opacity: 0.5;
  }

  50% {
    opacity: 1;
  }
}

.slide-preview {
  position: relative;
  z-index: 1;
  height: min(400px, 52vh);
  overflow: hidden;
  background: linear-gradient(135deg,
      rgba(255, 255, 255, 0.05) 0%,
      rgba(255, 255, 255, 0.01) 100%);
  border: 1px solid rgba(255, 240, 240, 0.215);
  box-shadow: 0 20px 60px rgba(0, 0, 0, 0.4);
  transition: box-shadow 0.4s ease;
}

.project-image {
  width: 100%;
  height: 100%;
  object-fit: cover;
  object-position: top;
}

.mockup {
  width: 100%;
  height: 100%;
  display: flex;
  flex-direction: column;
}

.mockup-bar {
  display: flex;
  align-items: center;
  gap: 0.4rem;
  padding: 0.7rem 1rem;
  background: rgba(255, 255, 255, 0.03);
  border-bottom: 0.5px solid rgba(255, 255, 255, 0.352);
  flex-shrink: 0;
}

.mdot {
  width: 8px;
  height: 8px;
  border-radius: 50%;
  background: var(--border-glass-hover);
}

.murl {
  flex: 1;
  height: 6px;
  border-radius: 3px;
  background: rgba(255, 255, 255, 0.06);
  margin-left: 0.5rem;
}

.mockup-body {
  flex: 1;
  display: flex;
  overflow: hidden;
}

.m-sidebar {
  width: 22%;
  padding: 0.75rem 0.5rem;
  border-right: 1px solid var(--border-glass);
  display: flex;
  flex-direction: column;
  gap: 0.45rem;
  flex-shrink: 0;
}

.ms-item {
  height: 8px;
  border-radius: 4px;
  background: rgba(255, 255, 255, 0.06);
}

.ms-item:first-child {
  background: var(--accent);
  opacity: 0.4;
}

.m-main {
  flex: 1;
  padding: 0.85rem 1rem;
  display: flex;
  flex-direction: column;
  gap: 0.55rem;
}

.m-line {
  height: 8px;
  border-radius: 4px;
  background: rgba(255, 255, 255, 0.07);
}

.m-cards {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 0.55rem;
  flex: 1;
  margin-top: 0.25rem;
}

.m-card {
  border-radius: 8px;
  background: linear-gradient(135deg, var(--accent), transparent);
  opacity: 0.12;
  min-height: 40px;
}

/* ── Skeleton ── */
.skel {
  background: linear-gradient(90deg,
      rgba(255, 255, 255, 0.04) 25%,
      rgba(255, 255, 255, 0.09) 50%,
      rgba(255, 255, 255, 0.04) 75%);
  background-size: 200% 100%;
  animation: shimmer 1.6s linear infinite;
  border-radius: 6px;
}

@keyframes shimmer {
  from {
    background-position: 200% 0;
  }

  to {
    background-position: -200% 0;
  }
}

.skel-h {
  height: 24px;
  width: 80%;
  margin-bottom: 0.5rem;
}

.skel-p {
  height: 14px;
  width: 60%;
}

.skel-label {
  height: 11px;
  width: 40%;
  margin-bottom: 0.4rem;
}

.skel-tech {
  height: 18px;
  width: 80%;
}

/* Skeleton preview */
.skel-preview-inner {
  padding: 0.75rem 1rem;
  height: 100%;
  display: flex;
  flex-direction: column;
  gap: 0.65rem;
  box-sizing: border-box;
}

.skel-mbar {
  height: 24px;
  width: 100%;
  flex-shrink: 0;
  border-radius: 4px;
}

.skel-mbody {
  flex: 1;
  display: flex;
  gap: 0.65rem;
  min-height: 0;
}

.skel-msidebar {
  width: 22%;
  border-radius: 6px;
  flex-shrink: 0;
  align-self: stretch;
}

.skel-mmain {
  flex: 1;
  display: flex;
  flex-direction: column;
  gap: 0.55rem;
}

.skel-mline {
  height: 10px;
  width: 75%;
  border-radius: 4px;
}

.skel-mline.short {
  width: 45%;
}

.skel-mblock {
  flex: 1;
  border-radius: 8px;
  min-height: 0;
}

/* ─── Footer ──────────────────────────────────────────────── */
.proj-footer {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 0.85rem 5vw;
  border-top: 1px solid var(--border-glass);
  flex-shrink: 0;
}

.proj-dots {
  display: flex;
  gap: 0.45rem;
  align-items: center;
}

.proj-dot {
  height: 5px;
  width: 5px;
  border-radius: 3px;
  background: var(--border-glass-hover);
  transition: width 0.3s ease, background 0.3s ease;
}

.proj-dot.active {
  width: 22px;
  background: var(--accent);
}

.scroll-hint {
  font-size: 0.78rem;
  color: var(--text-muted);
  letter-spacing: 0.06em;
}

.scroll-hint.done {
  color: var(--accent);
}

/* Fade transition for hint text */
.fade-hint-enter-active,
.fade-hint-leave-active {
  transition: opacity 0.3s ease;
}

.fade-hint-enter-from,
.fade-hint-leave-to {
  opacity: 0;
}

/* ─── Responsive ───────────────────────────────────────────── */
@media (max-width: 900px) {
  .proj-slide {
    grid-template-columns: 1fr;
    padding: 1.5rem 6vw 1rem;
    overflow-y: auto;
    align-items: start;
  }

  .slide-preview {
    height: 220px;
  }

  .wm-num {
    font-size: clamp(5rem, 28vw, 10rem);
    bottom: -0.05em;
  }
}

@media (max-width: 600px) {
  .proj-header {
    flex-direction: column;
    align-items: flex-start;
    gap: 0.75rem;
    padding: 5rem 6vw 1rem;
  }

  .slide-idx {
    font-size: 2rem;
  }

  .slide-divider {
    margin: 0;
  }
}
</style>
