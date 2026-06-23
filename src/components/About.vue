<script setup>
</script>

<template>
  <section id="about" class="about-section">
    <div class="container">
      <h2 class="section-title">Về <span class="text-gradient">Tôi</span></h2>
      <div class="about-grid">
        <div class="glass-card">
          <h3><i class="fa-solid fa-wand-magic-sparkles" style="color: var(--accent); margin-right: 10px;"></i> Đam Mê Thiết Kế</h3>
          <p>Tôi tin rằng một thiết kế tuyệt vời không chỉ nằm ở vẻ bề ngoài mà còn ở cảm giác mang lại. Tôi luôn hướng đến sự hoàn hảo trong từng pixel và các hiệu ứng chuyển động mượt mà.</p>
        </div>
        <div class="glass-card">
          <h3><i class="fa-brands fa-vuejs" style="color: #42b883; margin-right: 10px;"></i> Chuyên Gia Vue</h3>
          <p>Thành thạo Vue 3, Composition API, Pinia và Vite. Tôi chuyên xây dựng các ứng dụng web một trang (SPA) có khả năng mở rộng, dễ bảo trì và tối ưu hiệu suất.</p>
        </div>
        <div class="glass-card">
          <h3><i class="fa-solid fa-bolt" style="color: #f59e0b; margin-right: 10px;"></i> Tối Ưu Tốc Độ</h3>
          <p>Tốc độ là một tính năng thiết yếu. Tôi đảm bảo ứng dụng tải cực nhanh và hoạt động trơn tru trên mọi thiết bị và điều kiện mạng.</p>
        </div>
      </div>
    </div>
  </section>
</template>

<style scoped>
.about-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  gap: 2rem;
  margin-top: 4rem;
}

.glass-card h3 {
  font-size: 1.5rem;
  margin-bottom: 1rem;
  display: flex;
  align-items: center;
}

.glass-card p {
  color: var(--text-muted);
}
</style>
