<template>
  <div class="page">
    <h1>我的文章</h1>
    <div class="blog-content">
      <p>文章系統即將上線，敬請期待！</p>
      <p>未來這裡會展示我的技術筆記和心得分享。</p>
    </div>
  </div>
</template>

<script setup>
// 文章頁面組件
</script>

<style scoped>
.page {
  animation: fadeIn 0.3s ease-in;
}

@keyframes fadeIn {
  from { opacity: 0; transform: translateY(10px); }
  to { opacity: 1; transform: translateY(0); }
}

.blog-content p {
  line-height: 1.6;
  color: #555;
  margin-bottom: 15px;
}
</style>