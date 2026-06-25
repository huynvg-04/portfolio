<script setup>
import { ref } from 'vue';

const form = ref({ name: '', email: '', message: '' });
const status = ref('idle'); // idle | loading | success | error

const socials = [
  { icon: 'fa-brands fa-github',   href: 'https://github.com/huynvg-04',                      label: 'GitHub',   color: '#e2e8f0' },
  { icon: 'fa-brands fa-linkedin', href: 'https://www.linkedin.com/in/ngo-van-gia-huy04',     label: 'LinkedIn', color: '#0a66c2' },
  { icon: 'fa-brands fa-facebook', href: 'https://www.facebook.com/em.an.com.chua.UwU',       label: 'Facebook', color: '#1877f2' },
];

const submit = async () => {
  status.value = 'loading';
  // Simulate async send
  await new Promise(r => setTimeout(r, 1500));
  status.value = 'success';
  form.value = { name: '', email: '', message: '' };
  setTimeout(() => { status.value = 'idle'; }, 4000);
};
</script>

<template>
  <section id="contact" class="contact-section">
    <div class="bg-glow-2 bg-glow" style="position:absolute;"></div>
    <div class="container">
      <h2 class="section-title flip-in-left">Liên Hệ <span class="text-gradient">Với Tôi</span></h2>
      <p class="section-subtitle flip-in-left" data-flip-delay="150">
        Tôi hiện đang tìm kiếm cơ hội mới. Hộp thư của tôi luôn mở — hãy nhắn tin bất cứ lúc nào!
      </p>

      <div class="contact-layout">
        <!-- Left: Info -->
        <div class="contact-info flip-in-left" data-flip-delay="200">
          <div class="glass-card info-card">
            <h3>Hãy cùng nhau xây dựng<br><span class="text-gradient">điều tuyệt vời</span></h3>
            <p class="info-sub">
              Tôi có thể nhận dự án freelance và sẵn sàng tham gia vào đội nhóm có văn hóa phát triển tốt.
            </p>

            <div class="info-items">
              <div class="info-item">
                <div class="info-icon">
                  <i class="fa-solid fa-envelope"></i>
                </div>
                <div>
                  <span class="info-label">Email</span>
                  <a href="mailto:ngovangiahuy04@gmail.com" class="info-value">ngovangiahuy04@gmail.com</a>
                </div>
              </div>
              <div class="info-item">
                <div class="info-icon">
                  <i class="fa-solid fa-location-dot"></i>
                </div>
                <div>
                  <span class="info-label">Địa chỉ</span>
                  <span class="info-value">Hà Nội, Việt Nam</span>
                </div>
              </div>
              <div class="info-item">
                <div class="info-icon">
                  <i class="fa-solid fa-circle-check"></i>
                </div>
                <div>
                  <span class="info-label">Trạng thái</span>
                  <span class="info-value available">Sẵn sàng nhận việc</span>
                </div>
              </div>
            </div>

            <!-- Social Links -->
            <div class="social-row">
              <a
                v-for="s in socials" :key="s.label"
                :href="s.href" target="_blank" rel="noopener"
                :aria-label="s.label"
                class="social-big"
                :style="{ '--s-color': s.color }"
              >
                <i :class="s.icon"></i>
                <span>{{ s.label }}</span>
              </a>
            </div>
          </div>
        </div>

        <!-- Right: Form -->
        <div class="contact-form-wrap flip-in-right" data-flip-delay="300">
          <div class="glass-card form-card">
            <h3 class="form-title"><i class="fa-solid fa-paper-plane"></i> Gửi Tin Nhắn</h3>

            <form @submit.prevent="submit" class="contact-form" novalidate>
              <div class="field-group">
                <div class="field">
                  <label for="name">Tên của bạn</label>
                  <input id="name" v-model="form.name" type="text" placeholder="Nguyễn Văn A" required :disabled="status==='loading'" />
                </div>
                <div class="field">
                  <label for="email">Email</label>
                  <input id="email" v-model="form.email" type="email" placeholder="example@gmail.com" required :disabled="status==='loading'" />
                </div>
              </div>

              <div class="field">
                <label for="message">Tin nhắn</label>
                <textarea id="message" v-model="form.message" rows="5"
                  placeholder="Xin chào, tôi muốn hợp tác với bạn về dự án..." required
                  :disabled="status==='loading'"></textarea>
              </div>

              <!-- Submit -->
              <button type="submit" class="btn btn-primary submit-btn"
                :disabled="status === 'loading' || status === 'success'" id="contact-submit">
                <template v-if="status === 'idle'">
                  Gửi Tin Nhắn <i class="fa-solid fa-paper-plane"></i>
                </template>
                <template v-else-if="status === 'loading'">
                  <span class="spinner"></span> Đang gửi...
                </template>
                <template v-else-if="status === 'success'">
                  <i class="fa-solid fa-check"></i> Đã gửi thành công!
                </template>
              </button>
            </form>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<style scoped>
.contact-section { overflow: hidden; position: relative; }

/* ── Layout ── */
.contact-layout {
  display: grid;
  grid-template-columns: 1fr 1.2fr;
  gap: 2rem;
  align-items: start;
}

/* ── Info Card ── */
.info-card {
  height: 100%;
  display: flex;
  flex-direction: column;
  gap: 1.75rem;
  background: var(--card-gradient);
}

.info-card h3 {
  font-size: clamp(1.3rem, 3.5vw, 1.7rem);
  line-height: 1.3;
}

.info-sub {
  color: var(--text-muted);
  line-height: 1.7;
  font-size: 0.95rem;
}

.info-items {
  display: flex;
  flex-direction: column;
  gap: 1.25rem;
}

.info-item {
  display: flex;
  align-items: center;
  gap: 1rem;
}

.info-icon {
  width: 44px;
  height: 44px;
  border-radius: 12px;
  background: var(--accent-dim);
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 1.1rem;
  color: var(--accent);
  flex-shrink: 0;
}

.info-label {
  display: block;
  font-size: 0.75rem;
  color: var(--text-muted);
  margin-bottom: 2px;
  text-transform: uppercase;
  letter-spacing: 0.05em;
}

.info-value {
  font-size: 0.95rem;
  font-weight: 500;
  color: var(--text-main);
  text-decoration: none;
  transition: color 0.2s;
}

a.info-value:hover { color: var(--accent); }

.available {
  color: var(--neon-green) !important;
  display: flex;
  align-items: center;
  gap: 0.4rem;
}

.available::before {
  content: '';
  display: inline-block;
  width: 8px;
  height: 8px;
  border-radius: 50%;
  background: var(--neon-green);
  box-shadow: 0 0 8px var(--neon-green);
  animation: dot-pulse 2s infinite;
}

/* ── Social Row ── */
.social-row {
  display: flex;
  gap: 0.75rem;
  margin-top: auto;
  flex-wrap: wrap;
}

.social-big {
  display: inline-flex;
  align-items: center;
  gap: 0.5rem;
  padding: 0.6rem 1.1rem;
  border-radius: 50px;
  border: 1px solid var(--border-glass);
  background: var(--bg-glass);
  color: var(--text-muted);
  text-decoration: none;
  font-size: 0.85rem;
  font-weight: 500;
  transition: var(--transition);
}

.social-big:hover {
  color: var(--s-color, var(--accent));
  border-color: var(--s-color, var(--accent));
  background: var(--bg-glass-hover);
  transform: translateY(-3px);
}

/* ── Form ── */
.form-card { height: 100%; }

.form-title {
  font-size: 1.2rem;
  margin-bottom: 1.75rem;
  display: flex;
  align-items: center;
  gap: 0.6rem;
  color: var(--text-main);
}

.form-title i { color: var(--accent); }

.field-group {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 1rem;
}

.field {
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
  margin-bottom: 1rem;
}

label {
  font-size: 0.85rem;
  font-weight: 500;
  color: var(--text-muted);
}

input, textarea {
  margin-bottom: 0;
  border-radius: var(--radius-sm);
  font-size: 0.95rem;
}

textarea { resize: vertical; min-height: 120px; }

input:disabled, textarea:disabled { opacity: 0.6; cursor: not-allowed; }

.submit-btn {
  width: 100%;
  justify-content: center;
  font-size: 1rem;
  padding: 1rem 2rem;
}

.submit-btn:disabled { opacity: 0.75; cursor: not-allowed; }

/* ── Spinner ── */
.spinner {
  display: inline-block;
  width: 16px;
  height: 16px;
  border: 2px solid rgba(255,255,255,0.4);
  border-top-color: #fff;
  border-radius: 50%;
  animation: spin-slow 0.7s linear infinite;
}

/* ── Responsive ── */
/* Tablet ≤ 900px */
@media (max-width: 900px) {
  .contact-layout { grid-template-columns: 1fr; gap: 1.5rem; }
  .field-group    { grid-template-columns: 1fr; }
  .info-card      { height: auto; }
}

/* Mobile ≤ 768px */
@media (max-width: 768px) {
  .contact-layout { gap: 1.25rem; }
  .info-card      { gap: 1.5rem; }
  .info-items     { gap: 1rem; }
  .social-row     { gap: 0.6rem; }
  .social-big     { padding: 0.55rem 0.9rem; font-size: 0.82rem; }
  .form-title     { font-size: 1.1rem; }
  .submit-btn     { padding: 0.875rem 1.75rem; font-size: 0.95rem; }
}

/* Small Mobile ≤ 480px */
@media (max-width: 480px) {
  .info-sub   { font-size: 0.88rem; }
  .info-icon  { width: 40px; height: 40px; font-size: 1rem; }
  .info-label { font-size: 0.7rem; }
  .info-value { font-size: 0.88rem; }
  .social-big { padding: 0.5rem 0.8rem; font-size: 0.8rem; }
  .field-group { gap: 0.75rem; }
  .field      { gap: 0.4rem; margin-bottom: 0.75rem; }
  textarea    { min-height: 100px; }
}

/* Very Small ≤ 360px */
@media (max-width: 360px) {
  .social-row { flex-direction: column; align-items: flex-start; gap: 0.5rem; }
  .social-big { width: 100%; justify-content: center; }
  .info-item  { gap: 0.75rem; }
}
</style>
