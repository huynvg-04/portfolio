<script setup>
</script>

<template>
  <section id="contact" class="contact-section">
    <div class="bg-glow bottom-left"></div>
    <div class="container">
      <div class="contact-wrapper glass-card">
        <div class="contact-info">
          <h2>Hãy cùng nhau xây dựng <br><span class="text-gradient">những điều tuyệt vời</span>.</h2>
          <p class="contact-sub">Tôi hiện đang tìm kiếm cơ hội tham gia vào các dự án thú vị và có thể nhận việc tự do ngay.</p>
          
          <div class="info-items">
            <div class="info-item">
              <i class="fa-solid fa-envelope"></i>
              <span>ngovangiahuy04@gmail.com</span>
            </div>
            <div class="info-item">
              <i class="fa-solid fa-location-dot"></i>
              <span>Hà Nội, Việt Nam</span>
            </div>
          </div>
        </div>
        
        <form class="contact-form" @submit.prevent>
          <div class="form-group">
            <input type="text" placeholder="Tên của bạn" required />
          </div>
          <div class="form-group">
            <input type="email" placeholder="Email của bạn" required />
          </div>
          <div class="form-group">
            <textarea rows="4" placeholder="Nhập tin nhắn..." required></textarea>
          </div>
          <button type="submit" class="btn btn-primary w-full">Gửi Tin Nhắn</button>
        </form>
      </div>
    </div>
  </section>
</template>

<style scoped>
.contact-wrapper {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 4rem;
  padding: 4rem;
}

.contact-info h2 {
  font-size: 2.5rem;
  margin-bottom: 1.5rem;
  line-height: 1.3;
}

.contact-sub {
  color: var(--text-muted);
  font-size: 1.1rem;
  margin-bottom: 3rem;
}

.info-items {
  display: flex;
  flex-direction: column;
  gap: 1.5rem;
}

.info-item {
  display: flex;
  align-items: center;
  gap: 1rem;
  font-size: 1.1rem;
  color: var(--text-main);
}

.info-item i {
  color: var(--accent);
  font-size: 1.25rem;
}

.w-full {
  width: 100%;
}

@media (max-width: 992px) {
  .contact-wrapper {
    grid-template-columns: 1fr;
    padding: 2rem;
  }
}
</style>
