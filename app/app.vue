<template>
  <div class="app">
    <!-- 導航菜單 -->
    <nav class="navbar">
      <div class="nav-container">
        <div class="nav-logo">
          <h3>我的網站</h3>
        </div>
        
        <!-- 桌面版菜單 -->
        <div class="nav-menu">
          <button 
            :class="{ active: currentPage === 'home' }"
            @click="switchPage('home')"
          >
            首頁
          </button>
          <button 
            :class="{ active: currentPage === 'about' }"
            @click="switchPage('about')"
          >
            關於我
          </button>
          <button 
            :class="{ active: currentPage === 'blog' }"
            @click="switchPage('blog')"
          >
            文章
          </button>
        </div>
        
        <!-- 手機版菜單按鈕 -->
        <div class="nav-toggle" @click="toggleMobileMenu">
          <span></span>
          <span></span>
          <span></span>
        </div>
      </div>
      
      <!-- 手機版下拉菜單 -->
      <div class="mobile-menu" :class="{ show: isMobileMenuOpen }">
        <button @click="switchPage('home')">首頁</button>
        <button @click="switchPage('about')">關於我</button>
        <button @click="switchPage('blog')">文章</button>
      </div>
    </nav>

    <!-- 主要內容區域 -->
    <main class="main-content">
      <!-- 首頁內容 -->
      <div v-if="currentPage === 'home'" class="page">
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
      
      <!-- 關於頁面 -->
      <div v-if="currentPage === 'about'" class="page">
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
      
      <!-- 文章頁面 -->
      <div v-if="currentPage === 'blog'" class="page">
        <h1>我的文章</h1>
        <div class="blog-content">
          <p>文章系統即將上線，敬請期待！</p>
          <p>未來這裡會展示我的技術筆記和心得分享。</p>
        </div>
      </div>
    </main>
  </div>
</template>

<script setup>
import { ref } from 'vue'

// 響應式數據
const currentPage = ref('home')  // 當前顯示的頁面
const isMobileMenuOpen = ref(false)  // 手機菜單是否打開

// 切換頁面的函數
function switchPage(page) {
  currentPage.value = page
  isMobileMenuOpen.value = false  // 切換頁面時關閉手機菜單
}

// 切換手機菜單顯示/隱藏
function toggleMobileMenu() {
  isMobileMenuOpen.value = !isMobileMenuOpen.value
}
</script>

<style scoped>
/* 全局樣式 */
.app {
  min-height: 100vh;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
}

/* 導航樣式 */
.navbar {
  background: #fff;
  box-shadow: 0 2px 10px rgba(0,0,0,0.1);
  position: sticky;
  top: 0;
  z-index: 100;
}

.nav-container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 20px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  height: 60px;
}

.nav-logo h3 {
  color: #2c3e50;
  margin: 0;
}

.nav-menu {
  display: flex;
  gap: 20px;
}

.nav-menu button {
  background: none;
  border: none;
  padding: 8px 16px;
  cursor: pointer;
  border-radius: 4px;
  transition: all 0.3s ease;
  color: #555;
}

.nav-menu button:hover {
  background: #f8f9fa;
  color: #2c3e50;
}

.nav-menu button.active {
  background: #3498db;
  color: white;
}

/* 手機版菜單按鈕 */
.nav-toggle {
  display: none;
  flex-direction: column;
  cursor: pointer;
}

.nav-toggle span {
  width: 25px;
  height: 3px;
  background: #333;
  margin: 3px 0;
  transition: 0.3s;
}

/* 手機版下拉菜單 */
.mobile-menu {
  display: none;
  background: white;
  box-shadow: 0 2px 10px rgba(0,0,0,0.1);
  padding: 20px;
}

.mobile-menu.show {
  display: block;
}

.mobile-menu button {
  display: block;
  width: 100%;
  background: none;
  border: none;
  padding: 12px;
  text-align: left;
  cursor: pointer;
  border-radius: 4px;
  margin-bottom: 8px;
  color: #333;
}

.mobile-menu button:hover {
  background: #f8f9fa;
}

/* 主要內容 */
.main-content {
  max-width: 800px;
  margin: 0 auto;
  padding: 40px 20px;
}

.page {
  animation: fadeIn 0.3s ease-in;
}

@keyframes fadeIn {
  from { opacity: 0; transform: translateY(10px); }
  to { opacity: 1; transform: translateY(0); }
}

/* 首頁樣式 */
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

/* 關於頁面樣式 */
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

/* 文章頁面樣式 */
.blog-content p {
  line-height: 1.6;
  color: #555;
  margin-bottom: 15px;
}

/* 響應式設計 */
@media (max-width: 768px) {
  .nav-menu {
    display: none;
  }
  
  .nav-toggle {
    display: flex;
  }
  
  .hero-section h1 {
    font-size: 2rem;
  }
  
  .main-content {
    padding: 20px;
  }
}
</style>
