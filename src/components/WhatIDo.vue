<script setup>
import { ref, onMounted } from 'vue';

const activeCard = ref(null);
const isTouchDevice = ref(false);

const cards = [
  {
    id: 0,
    title: 'DEVELOP',
    description: 'Building modern, performant web applications from end to end. I design clean architectures and write maintainable code that scales.',
    tags: ['JavaScript', 'TypeScript', 'Vue.js', 'React', 'Node.js', 'Laravel', 'PHP', 'MySQL', 'CSS', 'Git'],
  },
  {
    id: 1,
    title: 'DESIGN',
    description: 'Crafting intuitive, visually compelling interfaces. I bridge the gap between engineering and design to ship experiences that feel as good as they look.',
    tags: ['Figma', 'UI Design', 'Prototyping', 'Animation', 'Responsive', 'Glassmorphism'],
  },
];

onMounted(() => {
  isTouchDevice.value = 'ontouchstart' in window || navigator.maxTouchPoints > 0;
});

const toggleCard = (id) => {
  if (!isTouchDevice.value) return;
  activeCard.value = activeCard.value === id ? null : id;
};
</script>

<template>
  <section id="whatido" class="whatido-section">
    <div class="whatido-inner container">

      <!-- Left: big heading -->
      <div class="whatido-heading">
        <h2 class="whatido-h2 fade-left" data-delay="0">
          W<span class="h2-italic">HAT</span>
          <div>I<span class="h2-accent"> DO</span></div>
        </h2>
      </div>

      <!-- Right: expandable cards -->
      <div class="whatido-cards">
        <div
          v-for="card in cards"
          :key="card.id"
          class="wi-card"
          :class="{
            'wi-card--active': !isTouchDevice || activeCard === card.id,
            'wi-card--touch-active': isTouchDevice && activeCard === card.id,
          }"
          @click="toggleCard(card.id)"
        >
          <!-- Dashed border SVG: top & bottom lines -->
          <div class="wi-border-h">
            <svg height="100%" width="100%">
              <line x1="0" y1="0" x2="100%" y2="0" stroke="rgba(255,255,255,0.25)" stroke-width="1.5" stroke-dasharray="6,6" />
              <line x1="0" y1="100%" x2="100%" y2="100%" stroke="rgba(255,255,255,0.25)" stroke-width="1.5" stroke-dasharray="6,6" />
            </svg>
          </div>

          <!-- Corner dots -->
          <span class="wi-corner wi-corner--tl"></span>
          <span class="wi-corner wi-corner--tr"></span>
          <span class="wi-corner wi-corner--bl"></span>
          <span class="wi-corner wi-corner--br"></span>

          <div class="wi-card-inner">
            <h3 class="wi-title">{{ card.title }}</h3>
            <div class="wi-body">
              <h4 class="wi-desc-label">Description</h4>
              <p class="wi-desc">{{ card.description }}</p>
              <h5 class="wi-tags-label">Skillset &amp; tools</h5>
              <div class="wi-tags">
                <span v-for="tag in card.tags" :key="tag" class="wi-tag">{{ tag }}</span>
              </div>
            </div>
            <!-- Arrow indicator -->
            <div class="wi-arrow" :class="{ 'wi-arrow--open': !isTouchDevice || activeCard === card.id }">
              <svg width="14" height="14" viewBox="0 0 14 14" fill="none">
                <path d="M7 1v12M1 7l6 6 6-6" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
              </svg>
            </div>
          </div>
        </div>
      </div>

    </div>
  </section>
</template>

<style scoped>
.whatido-section {
  padding: 6rem 0;
  position: relative;
}

.whatido-inner {
  display: flex;
  align-items: center;
  gap: 6rem;
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 2rem;
}

/* ── Left heading ── */
.whatido-heading {
  flex-shrink: 0;
  width: 40%;
}

.whatido-h2 {
  font-size: clamp(3.5rem, 7vw, 6rem);
  font-weight: 700;
  line-height: 0.95;
  letter-spacing: -0.02em;
  color: var(--text-main);
}

.h2-italic {
  font-style: italic;
  font-weight: 300;
  opacity: 0.75;
}

.h2-accent {
  color: var(--accent);
}

.whatido-cards {
  flex: 1;
  display: flex;
  flex-direction: column;
  position: relative;
  height: 480px;
  overflow: hidden;
}

/* Vertical dashed lines on left & right of the cards container */
.whatido-cards::before,
.whatido-cards::after {
  content: '';
  position: absolute;
  top: 0;
  bottom: 0;
  width: 1.5px;
  background-image: repeating-linear-gradient(
    to bottom,
    rgba(255, 255, 255, 0.2) 0px,
    rgba(255, 255, 255, 0.2) 6px,
    transparent 6px,
    transparent 12px
  );
}

.whatido-cards::before { left: 0; }
.whatido-cards::after  { right: 0; }

.wi-card {
  position: relative;
  padding: 2.5rem 3rem;
  overflow: hidden;
  cursor: default;
  flex: 1;
  transition: flex 0.55s cubic-bezier(0.4, 0, 0.2, 1),
              background 0.35s ease;
}

@media (hover: hover) {
  .wi-card:hover {
    flex: 3;
    background: rgba(255, 255, 255, 0.025);
  }

  .wi-card:not(:hover) {
    flex: 1;
  }
}

.wi-card--touch-active {
  flex: 3;
  background: rgba(255, 255, 255, 0.025);
}

/* Dashed H borders */
.wi-border-h {
  position: absolute;
  inset: 0;
  pointer-events: none;
  opacity: 0.6;
}

.wi-border-h svg {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
}

/* Corner squares */
.wi-corner {
  position: absolute;
  width: 8px;
  height: 8px;
  border: 2px solid rgba(255, 255, 255, 0.5);
}
.wi-corner--tl { top: -1px; left: -1px; border-right: none; border-bottom: none; }
.wi-corner--tr { top: -1px; right: -1px; border-left: none; border-bottom: none; }
.wi-corner--bl { bottom: -1px; left: -1px; border-right: none; border-top: none; }
.wi-corner--br { bottom: -1px; right: -1px; border-left: none; border-top: none; }

/* ── Card inner content ── */
.wi-card-inner {
  position: relative;
  z-index: 1;
  height: 100%;
}

.wi-title {
  font-size: 2rem;
  font-weight: 700;
  letter-spacing: 0.05em;
  margin-bottom: 0;
  color: var(--text-main);
}

.wi-body {
  overflow: hidden;
  max-height: 0;
  opacity: 0;
  transition: max-height 0.55s cubic-bezier(0.4, 0, 0.2, 1),
              opacity 0.4s ease 0.1s;
  margin-top: 0;
}

@media (hover: hover) {
  .wi-card:hover .wi-body {
    max-height: 400px;
    opacity: 1;
    margin-top: 1.5rem;
  }

  .wi-card:not(:hover) .wi-body {
    max-height: 0;
    opacity: 0;
    margin-top: 0;
  }
}

.wi-card--touch-active .wi-body {
  max-height: 400px;
  opacity: 1;
  margin-top: 1.5rem;
}

.wi-desc-label {
  font-size: 0.72rem;
  font-weight: 400;
  text-transform: uppercase;
  letter-spacing: 0.12em;
  color: var(--text-muted);
  margin-bottom: 0.5rem;
  opacity: 0.6;
}

.wi-desc {
  font-size: 0.95rem;
  font-weight: 300;
  color: var(--text-muted);
  line-height: 1.7;
  margin-bottom: 1.5rem;
  letter-spacing: 0.02em;
}

.wi-tags-label {
  font-size: 0.7rem;
  font-weight: 300;
  text-transform: uppercase;
  letter-spacing: 0.12em;
  color: var(--text-muted);
  opacity: 0.5;
  margin-bottom: 0.6rem;
}

.wi-tags {
  display: flex;
  flex-wrap: wrap;
  gap: 0.4rem;
}

.wi-tag {
  font-size: 0.78rem;
  padding: 0.25rem 0.75rem;
  background: rgba(255, 255, 255, 0.08);
  border: 1px solid rgba(255, 255, 255, 0.15);
  border-radius: 30px;
  color: var(--text-main);
  font-weight: 400;
}

/* Arrow */
.wi-arrow {
  position: absolute;
  bottom: 0;
  right: 0;
  width: 28px;
  height: 28px;
  display: flex;
  align-items: center;
  justify-content: center;
  color: rgba(255, 255, 255, 0.4);
  border: 1px solid rgba(255, 255, 255, 0.15);
  transition: transform 0.4s ease, color 0.3s ease;
}

.wi-arrow--open {
  transform: rotate(180deg);
  color: var(--accent);
}

/* ── Responsive ── */
@media (max-width: 960px) {
  .whatido-inner {
    flex-direction: column;
    gap: 3rem;
  }

  .whatido-heading {
    width: 100%;
  }

  .whatido-h2 {
    font-size: clamp(3rem, 12vw, 5rem);
  }

  .wi-card {
    cursor: pointer;
  }
}

@media (max-width: 480px) {
  .wi-card {
    padding: 2rem 1.5rem;
  }

  .wi-title {
    font-size: 1.5rem;
  }
}
</style>
