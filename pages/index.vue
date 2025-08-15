<template>
  <div class="page">
    <header class="hero-section">
      <h1>歡迎來到我的個人網站</h1>
      <p>iOS開發者 | 程式愛好者 | 技術文章作者</p>
    </header>
    
    <section class="intro-section">
      <h2>關於我</h2>
      <p>我是一位專注於iOS開發的程式設計師，熱愛分享技術心得和學習筆記。</p>
      <p>歡迎瀏覽我的文章和了解我的開發經歷。</p>
    </section>
  </div>
</template>

<script setup>
// 首頁組件
</script>

<style scoped>
.page {
  animation: fadeIn 0.3s ease-in;
}

@keyframes fadeIn {
  from { opacity: 0; transform: translateY(10px); }
  to { opacity: 1; transform: translateY(0); }
}

.hero-section {
  text-align: center;
  margin-bottom: 50px;
}

.hero-section h1 {
  font-size: 2.5rem;
  color: #2c3e50;
  margin-bottom: 10px;
}

.hero-section p {
  font-size: 1.2rem;
  color: #7f8c8d;
}

.intro-section h2 {
  color: #34495e;
  margin-bottom: 20px;
}

.intro-section p {
  line-height: 1.6;
  color: #555;
  margin-bottom: 15px;
}
</style>