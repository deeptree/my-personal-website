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
      
      <!-- 文章列表頁面 -->
      <div v-if="currentPage === 'blog'" class="page">
        <h1>我的文章</h1>
        <div class="articles-grid">
          <article 
            v-for="article in articles" 
            :key="article.id"
            class="article-card"
            @click="viewArticle(article.id)"
          >
            <div class="article-header">
              <h2>{{ article.title }}</h2>
              <div class="article-meta">
                <span class="date">{{ article.date }}</span>
                <span class="read-time">{{ article.readTime }}</span>
              </div>
            </div>
            
            <p class="article-summary">{{ article.summary }}</p>
            
            <div class="article-tags">
              <span 
                v-for="tag in article.tags" 
                :key="tag"
                class="tag"
              >
                {{ tag }}
              </span>
            </div>
            
            <div class="read-more">
              閱讀全文 →
            </div>
          </article>
        </div>
      </div>
      
      <!-- 文章詳情頁面 -->
      <div v-if="currentPage === 'article-detail' && selectedArticle" class="page">
        <div class="article-detail">
          <button class="back-btn" @click="backToArticles">
            ← 返回文章列表
          </button>
          
          <header class="article-detail-header">
            <h1>{{ selectedArticle.title }}</h1>
            <div class="article-detail-meta">
              <span class="date">{{ selectedArticle.date }}</span>
              <span class="read-time">{{ selectedArticle.readTime }}</span>
            </div>
            <div class="article-detail-tags">
              <span 
                v-for="tag in selectedArticle.tags" 
                :key="tag"
                class="tag"
              >
                {{ tag }}
              </span>
            </div>
          </header>
          
          <div class="article-content" v-html="formatContent(selectedArticle.content)">
          </div>
        </div>
      </div>
    </main>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'

// 響應式數據
const currentPage = ref('home')  // 當前顯示的頁面
const isMobileMenuOpen = ref(false)  // 手機菜單是否打開
const selectedArticleId = ref(null)  // 選中的文章ID

// 文章數據（模擬數據庫）
const articles = ref([
  {
    id: 1,
    title: "學習Vue.js的第一週心得",
    summary: "從iOS開發者的角度學習Vue.js，分享一些有趣的發現和對比...",
    content: `# 學習Vue.js的第一週心得

作為一個iOS開發者，剛開始學習Vue.js真的有很多相似的地方！

## 響應式數據 vs @State

Vue的響應式數據就像SwiftUI的@State：

\`\`\`javascript
// Vue
const isVisible = ref(false)
\`\`\`

\`\`\`swift
// SwiftUI  
@State var isVisible = false
\`\`\`

## 組件化思維

Vue的組件就像iOS的UIView，都是可重複使用的UI片段。

## 總結

學習Web開發讓我對跨平台開發有了新的理解！`,
    date: "2025-01-15",
    tags: ["Vue.js", "學習心得", "iOS開發"],
    readTime: "5 分鐘"
  },
  {
    id: 2,
    title: "響應式設計的重要性",
    summary: "為什麼每個網站都需要響應式設計？從手機優先的設計理念談起...",
    content: `# 響應式設計的重要性

現在超過60%的用戶使用手機瀏覽網站，響應式設計已經不是選項，而是必需品。

## 什麼是響應式設計？

響應式設計就是讓網站在不同螢幕尺寸下都能完美顯示：

- 📱 手機版：垂直佈局，大按鈕
- 💻 桌面版：水平佈局，豐富內容

## CSS Media Queries

\`\`\`css
@media (max-width: 768px) {
  .nav-menu {
    display: none;
  }
}
\`\`\`

## 最佳實踐

1. Mobile First 設計
2. 觸控友善的按鈕大小
3. 適當的字體大小`,
    date: "2025-01-10",
    tags: ["CSS", "響應式設計", "UX"],
    readTime: "3 分鐘"
  },
  {
    id: 3,
    title: "Git版本控制入門",
    summary: "學習Git的基本概念和常用指令，讓你的代碼管理更有條理...",
    content: `# Git版本控制入門

Git是每個開發者都必須掌握的工具，就像iOS開發中的Xcode內建版本控制。

## 基本概念

- **Repository**: 代碼倉庫
- **Commit**: 提交一個版本
- **Branch**: 分支開發
- **Merge**: 合併代碼

## 常用指令

\`\`\`bash
git init          # 初始化倉庫
git add .         # 添加所有文件
git commit -m "message"  # 提交變更
git push          # 推送到遠程
\`\`\`

## 最佳實踐

1. 經常提交小的變更
2. 寫清楚的commit訊息
3. 使用分支開發新功能`,
    date: "2025-01-05",
    tags: ["Git", "版本控制", "開發工具"],
    readTime: "4 分鐘"
  }
])

// 計算屬性：根據選中的文章ID找到文章詳情
const selectedArticle = computed(() => {
  return articles.value.find(article => article.id === selectedArticleId.value)
})

// 切換頁面的函數
function switchPage(page) {
  currentPage.value = page
  selectedArticleId.value = null  // 切換頁面時清除選中的文章
  isMobileMenuOpen.value = false  // 切換頁面時關閉手機菜單
}

// 查看文章詳情
function viewArticle(articleId) {
  selectedArticleId.value = articleId
  currentPage.value = 'article-detail'
}

// 返回文章列表
function backToArticles() {
  selectedArticleId.value = null
  currentPage.value = 'blog'
}

// 切換手機菜單顯示/隱藏
function toggleMobileMenu() {
  isMobileMenuOpen.value = !isMobileMenuOpen.value
}

// 格式化文章內容（簡單的Markdown轉HTML）
function formatContent(content) {
  return content
    .replace(/^# (.*$)/gm, '<h1>$1</h1>')
    .replace(/^## (.*$)/gm, '<h2>$1</h2>')
    .replace(/^### (.*$)/gm, '<h3>$1</h3>')
    .replace(/\*\*(.*?)\*\*/g, '<strong>$1</strong>')
    .replace(/\*(.*?)\*/g, '<em>$1</em>')
    .replace(/```(\w+)?\n([\s\S]*?)```/g, '<pre><code>$2</code></pre>')
    .replace(/`(.*?)`/g, '<code>$1</code>')
    .replace(/^- (.*$)/gm, '<li>$1</li>')
    .replace(/(<li>.*<\/li>)/gs, '<ul>$1</ul>')
    .replace(/^\d+\. (.*$)/gm, '<li>$1</li>')
    .replace(/\n\n/g, '</p><p>')
    .replace(/^(?!<[hul])/gm, '<p>')
    .replace(/(?<![>])$/gm, '</p>')
    .replace(/<p><\/p>/g, '')
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

/* 文章列表樣式 */
.articles-grid {
  display: grid;
  gap: 30px;
  margin-top: 30px;
}

.article-card {
  background: white;
  border-radius: 12px;
  padding: 25px;
  box-shadow: 0 4px 20px rgba(0,0,0,0.08);
  cursor: pointer;
  transition: all 0.3s ease;
  border: 1px solid #f0f0f0;
}

.article-card:hover {
  transform: translateY(-5px);
  box-shadow: 0 8px 30px rgba(0,0,0,0.15);
}

.article-header h2 {
  color: #2c3e50;
  margin-bottom: 10px;
  font-size: 1.4rem;
  line-height: 1.3;
}

.article-meta {
  display: flex;
  gap: 15px;
  margin-bottom: 15px;
  font-size: 0.9rem;
  color: #7f8c8d;
}

.article-summary {
  color: #555;
  line-height: 1.6;
  margin-bottom: 20px;
}

.article-tags {
  display: flex;
  flex-wrap: wrap;
  gap: 8px;
  margin-bottom: 15px;
}

.tag {
  background: #ecf0f1;
  color: #34495e;
  padding: 4px 12px;
  border-radius: 20px;
  font-size: 0.85rem;
}

.read-more {
  color: #3498db;
  font-weight: 500;
  display: flex;
  align-items: center;
  gap: 5px;
}

/* 文章詳情樣式 */
.article-detail {
  max-width: 800px;
  margin: 0 auto;
}

.back-btn {
  background: #f8f9fa;
  border: none;
  padding: 8px 16px;
  border-radius: 6px;
  cursor: pointer;
  color: #495057;
  margin-bottom: 30px;
  transition: background 0.3s ease;
}

.back-btn:hover {
  background: #e9ecef;
}

.article-detail-header {
  margin-bottom: 40px;
  padding-bottom: 20px;
  border-bottom: 1px solid #eee;
}

.article-detail-header h1 {
  color: #2c3e50;
  margin-bottom: 15px;
  font-size: 2rem;
  line-height: 1.2;
}

.article-detail-meta {
  display: flex;
  gap: 20px;
  margin-bottom: 15px;
  color: #7f8c8d;
}

.article-detail-tags {
  display: flex;
  flex-wrap: wrap;
  gap: 8px;
}

.article-content {
  line-height: 1.7;
  color: #333;
}

.article-content h1 {
  color: #2c3e50;
  margin: 30px 0 20px 0;
  font-size: 1.8rem;
}

.article-content h2 {
  color: #34495e;
  margin: 25px 0 15px 0;
  font-size: 1.4rem;
}

.article-content h3 {
  color: #34495e;
  margin: 20px 0 10px 0;
  font-size: 1.2rem;
}

.article-content p {
  margin-bottom: 16px;
}

.article-content ul {
  margin-bottom: 16px;
  padding-left: 20px;
}

.article-content li {
  margin-bottom: 5px;
}

.article-content code {
  background: #f1f3f4;
  padding: 2px 6px;
  border-radius: 4px;
  font-family: 'Monaco', 'Consolas', monospace;
  font-size: 0.9em;
}

.article-content pre {
  background: #f8f9fa;
  padding: 20px;
  border-radius: 8px;
  overflow-x: auto;
  margin: 20px 0;
}

.article-content pre code {
  background: none;
  padding: 0;
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
