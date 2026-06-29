<script setup>
const socials = [
  { label: 'Github', href: 'https://github.com/huynvg-04' },
  { label: 'Linkedin', href: 'https://www.linkedin.com/in/ngo-van-gia-huy04' },
  { label: 'Facebook', href: 'https://www.facebook.com/em.an.com.chua.UwU' },
  { label: 'Instagram', href: '#' },
];
</script>

<template>
  <section id="contact" class="contact-section">
    <div class="container contact-container">
      <h2 class="contact-title">CONTACT</h2>

      <div class="contact-grid">
        <!-- Column 1 -->
        <div class="contact-col">
          <div class="contact-item">
            <p class="contact-label">Email</p>
            <a href="mailto:ngovangiahuy04@gmail.com" class="contact-val">ngovangiahuy04@gmail.com</a>
          </div>
          <div class="contact-item">
            <p class="contact-label">Location</p>
            <p class="contact-val">Hanoi, Vietnam</p>
          </div>
        </div>

        <!-- Column 2 -->
        <div class="contact-col">
          <p class="contact-label">Social</p>
          <ul class="social-list">
            <li v-for="s in socials" :key="s.label">
              <a :href="s.href" target="_blank" rel="noopener">
                {{ s.label }} <span class="arrow">↗</span>
              </a>
            </li>
          </ul>
        </div>

        <!-- Column 3 -->
        <div class="contact-col footer-col">
          <p class="footer-text">Designed and Developed</p>
          <p class="footer-text">by <span class="author-name">Ngo Van Gia Huy</span></p>
          <p class="copyright">© 2026</p>
        </div>
      </div>
    </div>
  </section>
</template>

<style scoped>
.contact-section {
  padding: 8rem 0 4rem;
  background: var(--bg-dark);
  position: relative;
  overflow: hidden;
}

.contact-title {
  font-size: clamp(3rem, 6vw, 4.5rem);
  font-weight: 300;
  letter-spacing: -0.02em;
  margin-bottom: 4rem;
  color: var(--text-main);
}

.contact-grid {
  display: flex;
  justify-content: space-between;
  gap: 2rem;
  align-items: flex-start;
  flex-wrap: wrap;
}

.contact-col {
  display: flex;
  flex-direction: column;
  gap: 2rem;
}

.contact-item {
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
}

.contact-label {
  color: var(--text-muted);
  font-size: 0.85rem;
  font-weight: 500;
}

.contact-val {
  color: var(--text-main);
  font-size: 1.05rem;
  text-decoration: none;
  font-weight: 500;
  transition: color 0.3s;
}

a.contact-val:hover {
  color: var(--accent);
}

.social-list {
  list-style: none;
  padding: 0;
  margin: 0;
  display: flex;
  flex-direction: column;
}

.social-list li {
  border-bottom: 0.5px solid rgba(255, 255, 255, 0.2);
}

.social-list a {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 0.2rem 0;
  color: var(--text-main);
  text-decoration: none;
  font-size: 1.2rem;
  font-weight: 500;
  transition: color 0.3s;
}

.social-list a:hover {
  color: var(--accent);
}

.arrow {
  font-family: var(--font-code);
  font-size: 1.2rem;
  font-weight: 400;
}

.footer-col {
  gap: 0.5rem;
}

.footer-text {
  color: var(--text-main);
  font-size: 1.05rem;
  font-weight: 500;
}

.author-name {
  color: var(--accent);
}

.copyright {
  margin-top: 1.5rem;
  color: var(--text-muted);
  font-size: 1rem;
  display: flex;
  align-items: center;
  gap: 0.5rem;
}

@media (max-width: 768px) {
  .contact-grid {
    grid-template-columns: 1fr;
    gap: 3rem;
  }
}
</style>
