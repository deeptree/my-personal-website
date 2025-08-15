<template>
  <div class="page">
    <h1>關於我</h1>
    <div class="about-content">
      <h2>技能專長</h2>
      <ul>
        <li>iOS開發 (Swift, SwiftUI, UIKit)</li>
        <li>現在學習Web開發 (Vue.js, Nuxt)</li>
        <li>版本控制 (Git)</li>
      </ul>
      
      <h2>開發經歷</h2>
      <p>專注於iOS應用開發，對使用者體驗和程式碼品質有高度要求。</p>
      <p>目前正在學習現代Web開發技術，希望能夠全端發展。</p>
    </div>
  </div>
</template>

<script setup>
// 關於頁面組件
</script>

<style scoped>
.page {
  animation: fadeIn 0.3s ease-in;
}

@keyframes fadeIn {
  from { opacity: 0; transform: translateY(10px); }
  to { opacity: 1; transform: translateY(0); }
}

.about-content h2 {
  color: #2c3e50;
  margin-top: 30px;
  margin-bottom: 15px;
}

.about-content ul {
  padding-left: 20px;
}

.about-content li {
  margin-bottom: 8px;
  color: #555;
}

.about-content p {
  line-height: 1.6;
  color: #555;
  margin-bottom: 15px;
}
</style>