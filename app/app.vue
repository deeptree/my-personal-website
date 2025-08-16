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
          <button 
            :class="{ active: currentPage === 'contact' }"
            @click="switchPage('contact')"
          >
            聯絡
          </button>
          <button 
            class="theme-toggle"
            @click="toggleDarkMode"
            :title="isDarkMode ? '切換到淺色模式' : '切換到深色模式'"
          >
            {{ isDarkMode ? '☀️' : '🌙' }}
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
        <button @click="switchPage('contact')">聯絡</button>
        <button @click="toggleDarkMode" class="theme-toggle-mobile">
          {{ isDarkMode ? '☀️ 淺色模式' : '🌙 深色模式' }}
        </button>
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
        <div class="blog-header">
          <h1>我的文章</h1>
          
          <!-- 搜尋框 -->
          <div class="search-container">
            <div class="search-box">
              <span class="search-icon">🔍</span>
              <input 
                v-model="searchQuery"
                type="text" 
                placeholder="搜尋文章標題、內容或標籤..."
                class="search-input"
              />
              <button 
                v-if="searchQuery"
                @click="searchQuery = ''"
                class="clear-search"
              >
                ✕
              </button>
            </div>
            
            <!-- 搜尋結果提示 -->
            <div v-if="searchQuery" class="search-results-info">
              找到 {{ filteredArticles.length }} 篇文章
              <span v-if="filteredArticles.length === 0">
                ，嘗試使用不同的關鍵字
              </span>
            </div>
          </div>
        </div>
        
        <!-- 文章網格 -->
        <div class="articles-grid" v-if="filteredArticles.length > 0">
          <article 
            v-for="article in filteredArticles" 
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
        
        <!-- 空搜尋結果 -->
        <div v-else-if="searchQuery" class="empty-search">
          <div class="empty-icon">📝</div>
          <h3>沒有找到相關文章</h3>
          <p>嘗試使用不同的關鍵字，或者</p>
          <button @click="searchQuery = ''" class="reset-search-btn">
            瀏覽所有文章
          </button>
        </div>
      </div>
      
      <!-- 聯絡頁面 -->
      <div v-if="currentPage === 'contact'" class="page">
        <div class="contact-container">
          <div class="contact-header">
            <h1>聯絡我</h1>
            <p>有任何問題或想法？歡迎隨時聯絡我！</p>
          </div>
          
          <!-- 成功訊息 -->
          <div v-if="submitSuccess" class="success-message">
            <span class="success-icon">✓</span>
            訊息發送成功！我會盡快回覆您。
          </div>
          
          <div class="contact-content">
            <!-- 聯絡表單 -->
            <div class="contact-form-section">
              <h2>發送訊息</h2>
              
              <form @submit.prevent="submitContactForm" class="contact-form">
                <!-- 姓名 -->
                <div class="form-group">
                  <label for="name">姓名 *</label>
                  <input 
                    id="name"
                    v-model="contactForm.name"
                    type="text" 
                    :class="{ error: formErrors.name }"
                    placeholder="請輸入您的姓名"
                  />
                  <span v-if="formErrors.name" class="error-text">{{ formErrors.name }}</span>
                </div>
                
                <!-- 信箱 -->
                <div class="form-group">
                  <label for="email">信箱 *</label>
                  <input 
                    id="email"
                    v-model="contactForm.email"
                    type="email" 
                    :class="{ error: formErrors.email }"
                    placeholder="請輸入您的信箱"
                  />
                  <span v-if="formErrors.email" class="error-text">{{ formErrors.email }}</span>
                </div>
                
                <!-- 主旨 -->
                <div class="form-group">
                  <label for="subject">主旨 *</label>
                  <input 
                    id="subject"
                    v-model="contactForm.subject"
                    type="text" 
                    :class="{ error: formErrors.subject }"
                    placeholder="請簡述您的聯絡原因"
                  />
                  <span v-if="formErrors.subject" class="error-text">{{ formErrors.subject }}</span>
                </div>
                
                <!-- 訊息 -->
                <div class="form-group">
                  <label for="message">訊息 *</label>
                  <textarea 
                    id="message"
                    v-model="contactForm.message"
                    :class="{ error: formErrors.message }"
                    placeholder="請詳細描述您想說的話..."
                    rows="6"
                  ></textarea>
                  <span v-if="formErrors.message" class="error-text">{{ formErrors.message }}</span>
                </div>
                
                <!-- 提交按鈕 -->
                <button 
                  type="submit" 
                  class="submit-btn"
                  :disabled="isSubmitting"
                >
                  <span v-if="isSubmitting" class="loading-spinner">⏳</span>
                  {{ isSubmitting ? '發送中...' : '發送訊息' }}
                </button>
              </form>
            </div>
            
            <!-- 聯絡資訊 -->
            <div class="contact-info-section">
              <h2>其他聯絡方式</h2>
              
              <div class="contact-methods">
                <div class="contact-method">
                  <span class="method-icon">📧</span>
                  <div class="method-content">
                    <h3>信箱</h3>
                    <p>your.email@example.com</p>
                  </div>
                </div>
                
                <div class="contact-method">
                  <span class="method-icon">💼</span>
                  <div class="method-content">
                    <h3>LinkedIn</h3>
                    <p>linkedin.com/in/yourprofile</p>
                  </div>
                </div>
                
                <div class="contact-method">
                  <span class="method-icon">💻</span>
                  <div class="method-content">
                    <h3>GitHub</h3>
                    <p>github.com/yourusername</p>
                  </div>
                </div>
              </div>
              
              <div class="contact-note">
                <p>💡 <strong>提示：</strong>通常我會在24小時內回覆。如果是緊急事務，建議直接通過信箱聯絡。</p>
              </div>
            </div>
          </div>
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
import { ref, computed, onMounted } from 'vue'

// 響應式數據
const currentPage = ref('home')  // 當前顯示的頁面
const isMobileMenuOpen = ref(false)  // 手機菜單是否打開
const selectedArticleId = ref(null)  // 選中的文章ID
const isDarkMode = ref(false)  // 深色模式狀態
const searchQuery = ref('')  // 搜尋關鍵字

// 聯絡表單數據
const contactForm = ref({
  name: '',
  email: '',
  subject: '',
  message: ''
})
const formErrors = ref({})
const isSubmitting = ref(false)
const submitSuccess = ref(false)

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

// 計算屬性：根據搜尋關鍵字過濾文章
const filteredArticles = computed(() => {
  if (!searchQuery.value.trim()) {
    return articles.value
  }
  
  const query = searchQuery.value.toLowerCase()
  return articles.value.filter(article => 
    article.title.toLowerCase().includes(query) ||
    article.summary.toLowerCase().includes(query) ||
    article.content.toLowerCase().includes(query) ||
    article.tags.some(tag => tag.toLowerCase().includes(query))
  )
})

// 切換頁面的函數
function switchPage(page) {
  currentPage.value = page
  selectedArticleId.value = null  // 切換頁面時清除選中的文章
  searchQuery.value = ''  // 切換頁面時清除搜尋
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

// 切換深色模式
function toggleDarkMode() {
  isDarkMode.value = !isDarkMode.value
  // 檢查是否在瀏覽器環境
  if (typeof window !== 'undefined') {
    // 保存到瀏覽器的localStorage
    localStorage.setItem('darkMode', isDarkMode.value.toString())
    // 更新document的class來應用全局樣式
    document.documentElement.classList.toggle('dark', isDarkMode.value)
  }
}

// 初始化深色模式（從localStorage讀取用戶偏好）
function initDarkMode() {
  // 檢查是否在瀏覽器環境（客戶端）
  if (typeof window !== 'undefined') {
    const saved = localStorage.getItem('darkMode')
    if (saved !== null) {
      isDarkMode.value = saved === 'true'
    } else {
      // 如果沒有保存的偏好，檢查系統偏好
      isDarkMode.value = window.matchMedia('(prefers-color-scheme: dark)').matches
    }
    document.documentElement.classList.toggle('dark', isDarkMode.value)
  }
}

// 表單驗證函數
function validateForm() {
  const errors = {}
  
  if (!contactForm.value.name.trim()) {
    errors.name = '請輸入姓名'
  }
  
  if (!contactForm.value.email.trim()) {
    errors.email = '請輸入信箱'
  } else if (!/^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(contactForm.value.email)) {
    errors.email = '請輸入有效的信箱格式'
  }
  
  if (!contactForm.value.subject.trim()) {
    errors.subject = '請輸入主旨'
  }
  
  if (!contactForm.value.message.trim()) {
    errors.message = '請輸入訊息內容'
  } else if (contactForm.value.message.trim().length < 10) {
    errors.message = '訊息內容至少需要10個字符'
  }
  
  formErrors.value = errors
  return Object.keys(errors).length === 0
}

// 提交表單函數
async function submitContactForm() {
  if (!validateForm()) {
    return
  }
  
  isSubmitting.value = true
  
  // 模擬API請求延遲
  await new Promise(resolve => setTimeout(resolve, 2000))
  
  // 這裡在真實應用中會發送到後端API
  console.log('表單提交：', contactForm.value)
  
  // 重置表單
  contactForm.value = {
    name: '',
    email: '',
    subject: '',
    message: ''
  }
  formErrors.value = {}
  isSubmitting.value = false
  submitSuccess.value = true
  
  // 3秒後隱藏成功訊息
  setTimeout(() => {
    submitSuccess.value = false
  }, 3000)
}

// 組件掛載時初始化深色模式
onMounted(() => {
  initDarkMode()
})

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
/* CSS變數定義 */
:global(:root) {
  /* 淺色模式 */
  --bg-primary: #ffffff;
  --bg-secondary: #f8f9fa;
  --text-primary: #2c3e50;
  --text-secondary: #555555;
  --text-muted: #7f8c8d;
  --border-color: #e9ecef;
  --shadow-light: rgba(0,0,0,0.08);
  --shadow-heavy: rgba(0,0,0,0.15);
  --nav-bg: #ffffff;
  --card-bg: #ffffff;
  --code-bg: #f1f3f4;
  --pre-bg: #f8f9fa;
}

:global(.dark) {
  /* 深色模式 */
  --bg-primary: #1a1a1a;
  --bg-secondary: #2d2d2d;
  --text-primary: #e4e4e7;
  --text-secondary: #a1a1aa;
  --text-muted: #71717a;
  --border-color: #404040;
  --shadow-light: rgba(0,0,0,0.3);
  --shadow-heavy: rgba(0,0,0,0.5);
  --nav-bg: #1f1f1f;
  --card-bg: #262626;
  --code-bg: #374151;
  --pre-bg: #1f2937;
}

/* 全局樣式 */
.app {
  min-height: 100vh;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  background-color: var(--bg-primary);
  color: var(--text-primary);
  transition: background-color 0.3s ease, color 0.3s ease;
}

/* 導航樣式 */
.navbar {
  background: var(--nav-bg);
  box-shadow: 0 2px 10px var(--shadow-light);
  position: sticky;
  top: 0;
  z-index: 100;
  transition: background-color 0.3s ease;
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
  color: var(--text-primary);
  margin: 0;
  transition: color 0.3s ease;
}

.nav-menu {
  display: flex;
  gap: 20px;
  align-items: center;
}

.nav-menu button {
  background: none;
  border: none;
  padding: 8px 16px;
  cursor: pointer;
  border-radius: 4px;
  transition: all 0.3s ease;
  color: var(--text-secondary);
}

.nav-menu button:hover {
  background: var(--bg-secondary);
  color: var(--text-primary);
}

.nav-menu button.active {
  background: #3498db;
  color: white;
}

.theme-toggle {
  font-size: 1.2rem;
  padding: 6px 12px !important;
  border-radius: 50% !important;
}

.theme-toggle:hover {
  transform: scale(1.1);
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
  background: var(--text-primary);
  margin: 3px 0;
  transition: 0.3s;
}

/* 手機版下拉菜單 */
.mobile-menu {
  display: none;
  background: var(--nav-bg);
  box-shadow: 0 2px 10px var(--shadow-light);
  padding: 20px;
  transition: background-color 0.3s ease;
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
  color: var(--text-primary);
  transition: all 0.3s ease;
}

.mobile-menu button:hover {
  background: var(--bg-secondary);
}

.theme-toggle-mobile {
  border-top: 1px solid var(--border-color);
  margin-top: 10px !important;
  padding-top: 20px !important;
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
  color: var(--text-primary);
  margin-bottom: 10px;
  transition: color 0.3s ease;
}

.hero-section p {
  font-size: 1.2rem;
  color: var(--text-muted);
  transition: color 0.3s ease;
}

.intro-section h2 {
  color: var(--text-primary);
  margin-bottom: 20px;
  transition: color 0.3s ease;
}

.intro-section p {
  line-height: 1.6;
  color: var(--text-secondary);
  margin-bottom: 15px;
  transition: color 0.3s ease;
}

/* 關於頁面樣式 */
.about-content h2 {
  color: var(--text-primary);
  margin-top: 30px;
  margin-bottom: 15px;
  transition: color 0.3s ease;
}

.about-content ul {
  padding-left: 20px;
}

.about-content li {
  margin-bottom: 8px;
  color: var(--text-secondary);
  transition: color 0.3s ease;
}

.about-content p {
  line-height: 1.6;
  color: var(--text-secondary);
  margin-bottom: 15px;
  transition: color 0.3s ease;
}

/* 聯絡頁面樣式 */
.contact-container {
  max-width: 1000px;
  margin: 0 auto;
}

.contact-header {
  text-align: center;
  margin-bottom: 50px;
}

.contact-header h1 {
  color: var(--text-primary);
  margin-bottom: 15px;
  transition: color 0.3s ease;
}

.contact-header p {
  color: var(--text-muted);
  font-size: 1.1rem;
  transition: color 0.3s ease;
}

.success-message {
  background: #d4edda;
  color: #155724;
  padding: 15px 20px;
  border-radius: 8px;
  margin-bottom: 30px;
  display: flex;
  align-items: center;
  gap: 10px;
  animation: slideDown 0.3s ease;
}

.success-icon {
  background: #28a745;
  color: white;
  width: 20px;
  height: 20px;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 0.8rem;
}

@keyframes slideDown {
  from { opacity: 0; transform: translateY(-10px); }
  to { opacity: 1; transform: translateY(0); }
}

.contact-content {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 60px;
  align-items: start;
}

.contact-form-section h2,
.contact-info-section h2 {
  color: var(--text-primary);
  margin-bottom: 25px;
  font-size: 1.5rem;
  transition: color 0.3s ease;
}

/* 表單樣式 */
.contact-form {
  display: flex;
  flex-direction: column;
  gap: 25px;
}

.form-group {
  display: flex;
  flex-direction: column;
}

.form-group label {
  color: var(--text-primary);
  margin-bottom: 8px;
  font-weight: 500;
  transition: color 0.3s ease;
}

.form-group input,
.form-group textarea {
  background: var(--card-bg);
  border: 2px solid var(--border-color);
  border-radius: 8px;
  padding: 12px 16px;
  font-size: 1rem;
  color: var(--text-primary);
  transition: all 0.3s ease;
  resize: vertical;
}

.form-group input:focus,
.form-group textarea:focus {
  outline: none;
  border-color: #3498db;
  box-shadow: 0 0 0 3px rgba(52, 152, 219, 0.1);
}

.form-group input.error,
.form-group textarea.error {
  border-color: #e74c3c;
}

.form-group input::placeholder,
.form-group textarea::placeholder {
  color: var(--text-muted);
}

.error-text {
  color: #e74c3c;
  font-size: 0.9rem;
  margin-top: 5px;
}

.submit-btn {
  background: #3498db;
  color: white;
  border: none;
  padding: 15px 30px;
  border-radius: 8px;
  font-size: 1rem;
  cursor: pointer;
  transition: all 0.3s ease;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
}

.submit-btn:hover:not(:disabled) {
  background: #2980b9;
  transform: translateY(-2px);
}

.submit-btn:disabled {
  background: var(--text-muted);
  cursor: not-allowed;
  transform: none;
}

/* 聯絡資訊樣式 */
.contact-methods {
  display: flex;
  flex-direction: column;
  gap: 25px;
  margin-bottom: 30px;
}

.contact-method {
  display: flex;
  align-items: center;
  gap: 15px;
  padding: 20px;
  background: var(--card-bg);
  border-radius: 12px;
  border: 1px solid var(--border-color);
  transition: all 0.3s ease;
}

.contact-method:hover {
  transform: translateY(-2px);
  box-shadow: 0 4px 20px var(--shadow-light);
}

.method-icon {
  font-size: 1.5rem;
  width: 50px;
  height: 50px;
  display: flex;
  align-items: center;
  justify-content: center;
  background: var(--bg-secondary);
  border-radius: 50%;
}

.method-content h3 {
  color: var(--text-primary);
  margin-bottom: 5px;
  font-size: 1.1rem;
  transition: color 0.3s ease;
}

.method-content p {
  color: var(--text-muted);
  margin: 0;
  transition: color 0.3s ease;
}

.contact-note {
  background: var(--bg-secondary);
  padding: 20px;
  border-radius: 8px;
  border-left: 4px solid #3498db;
}

.contact-note p {
  color: var(--text-secondary);
  margin: 0;
  transition: color 0.3s ease;
}

/* 文章頁面標題 */
.blog-header {
  margin-bottom: 40px;
}

.blog-header h1 {
  color: var(--text-primary);
  margin-bottom: 30px;
  transition: color 0.3s ease;
}

/* 搜尋容器 */
.search-container {
  max-width: 500px;
}

.search-box {
  position: relative;
  display: flex;
  align-items: center;
  background: var(--card-bg);
  border: 2px solid var(--border-color);
  border-radius: 12px;
  padding: 0;
  transition: all 0.3s ease;
}

.search-box:focus-within {
  border-color: #3498db;
  box-shadow: 0 0 0 3px rgba(52, 152, 219, 0.1);
}

.search-icon {
  padding: 15px;
  color: var(--text-muted);
  font-size: 1.1rem;
}

.search-input {
  flex: 1;
  border: none;
  background: transparent;
  padding: 15px 10px;
  font-size: 1rem;
  color: var(--text-primary);
  outline: none;
}

.search-input::placeholder {
  color: var(--text-muted);
}

.clear-search {
  background: none;
  border: none;
  padding: 15px;
  color: var(--text-muted);
  cursor: pointer;
  border-radius: 50%;
  transition: all 0.3s ease;
}

.clear-search:hover {
  background: var(--bg-secondary);
  color: var(--text-primary);
}

.search-results-info {
  margin-top: 15px;
  color: var(--text-muted);
  font-size: 0.9rem;
}

/* 空搜尋結果 */
.empty-search {
  text-align: center;
  padding: 60px 20px;
  color: var(--text-secondary);
}

.empty-icon {
  font-size: 4rem;
  margin-bottom: 20px;
  opacity: 0.5;
}

.empty-search h3 {
  color: var(--text-primary);
  margin-bottom: 10px;
  transition: color 0.3s ease;
}

.empty-search p {
  margin-bottom: 25px;
  transition: color 0.3s ease;
}

.reset-search-btn {
  background: #3498db;
  color: white;
  border: none;
  padding: 12px 24px;
  border-radius: 8px;
  cursor: pointer;
  font-size: 1rem;
  transition: background 0.3s ease;
}

.reset-search-btn:hover {
  background: #2980b9;
}

/* 文章列表樣式 */
.articles-grid {
  display: grid;
  gap: 30px;
  margin-top: 30px;
}

.article-card {
  background: var(--card-bg);
  border-radius: 12px;
  padding: 25px;
  box-shadow: 0 4px 20px var(--shadow-light);
  cursor: pointer;
  transition: all 0.3s ease;
  border: 1px solid var(--border-color);
}

.article-card:hover {
  transform: translateY(-5px);
  box-shadow: 0 8px 30px var(--shadow-heavy);
}

.article-header h2 {
  color: var(--text-primary);
  margin-bottom: 10px;
  font-size: 1.4rem;
  line-height: 1.3;
  transition: color 0.3s ease;
}

.article-meta {
  display: flex;
  gap: 15px;
  margin-bottom: 15px;
  font-size: 0.9rem;
  color: var(--text-muted);
  transition: color 0.3s ease;
}

.article-summary {
  color: var(--text-secondary);
  line-height: 1.6;
  margin-bottom: 20px;
  transition: color 0.3s ease;
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
  color: var(--text-secondary);
  transition: color 0.3s ease;
}

.article-content h1 {
  color: var(--text-primary);
  margin: 30px 0 20px 0;
  font-size: 1.8rem;
  transition: color 0.3s ease;
}

.article-content h2 {
  color: var(--text-primary);
  margin: 25px 0 15px 0;
  font-size: 1.4rem;
  transition: color 0.3s ease;
}

.article-content h3 {
  color: var(--text-primary);
  margin: 20px 0 10px 0;
  font-size: 1.2rem;
  transition: color 0.3s ease;
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
  background: var(--code-bg);
  color: var(--text-primary);
  padding: 2px 6px;
  border-radius: 4px;
  font-family: 'Monaco', 'Consolas', monospace;
  font-size: 0.9em;
  transition: all 0.3s ease;
}

.article-content pre {
  background: var(--pre-bg);
  padding: 20px;
  border-radius: 8px;
  overflow-x: auto;
  margin: 20px 0;
  transition: background-color 0.3s ease;
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
  
  /* 聯絡頁面手機版 */
  .contact-content {
    grid-template-columns: 1fr;
    gap: 40px;
  }
  
  .contact-methods {
    gap: 20px;
  }
  
  .contact-method {
    padding: 15px;
  }
  
  .method-icon {
    width: 40px;
    height: 40px;
    font-size: 1.2rem;
  }
}
</style>
