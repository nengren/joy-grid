# Joy Grid 分享功能标准配置
# 所有游戏必须包含以下配置，不可遗漏

## 1. HEAD 区域 — OG 标签（必须）

每个游戏页面的 `<head>` 中必须包含以下标签：

```html
<!-- Open Graph (Facebook/LinkedIn/通用) -->
<meta property="og:title" content="Play [游戏名] Online Free - Joy Grid">
<meta property="og:description" content="[游戏一句话描述]">
<meta property="og:image" content="https://joy-grid.com/og-[游戏名].png">
<meta property="og:url" content="https://joy-grid.com/[游戏名].html">
<meta property="og:type" content="website">
<meta property="og:site_name" content="Joy Grid">

<!-- Twitter Card -->
<meta name="twitter:card" content="summary_large_image">
<meta name="twitter:title" content="Play [游戏名] Online Free - Joy Grid">
<meta name="twitter:description" content="[游戏一句话描述]">
<meta name="twitter:image" content="https://joy-grid.com/og-[游戏名].png">
```

**注意：og:image 必须是 PNG 格式，1200x630 像素。SVG 不被 Twitter/Facebook 支持。**

## 2. CSS — 分享按钮样式（复制到 `<style>` 中）

```css
.share-section{margin-top:20px;padding-top:20px;border-top:1px solid #eee}
.share-label{font-size:0.85rem;color:var(--text-soft);margin-bottom:10px;font-weight:600}
.share-btns{display:flex;gap:8px;justify-content:center;flex-wrap:wrap}
.share-btn{width:44px;height:44px;border:none;border-radius:12px;display:flex;align-items:center;justify-content:center;cursor:pointer;transition:transform 0.15s;font-size:1.2rem;color:#fff;font-weight:700;font-family:'Nunito',sans-serif;-webkit-tap-highlight-color:transparent}
.share-btn:hover{transform:scale(1.1)}
.share-btn:active{transform:scale(0.95)}
.share-btn.twitter{background:#000}
.share-btn.facebook{background:#1877F2;font-size:1.4rem}
.share-btn.whatsapp{background:#25D366}
.share-btn.native-share{background:var(--accent);font-size:1.3rem}
.share-btn.copy-link{background:#6b6560;font-size:1.2rem}
.share-toast{display:none;position:fixed;bottom:80px;left:50%;transform:translateX(-50%);background:#333;color:#fff;padding:10px 24px;border-radius:10px;font-size:0.9rem;font-weight:600;z-index:300}
.share-toast.show{display:block}
```

## 3. HTML — 分享按钮（放在结果弹窗内）

```html
<div class="share-section">
  <div class="share-label">Challenge your friends</div>
  <div class="share-btns">
    <button class="share-btn native-share" id="nativeShareBtn" onclick="nativeShare()">📤</button>
    <button class="share-btn twitter" onclick="shareTwitter()">𝕏</button>
    <button class="share-btn facebook" onclick="shareFacebook()">f</button>
    <button class="share-btn whatsapp" onclick="shareWhatsApp()">WA</button>
    <button class="share-btn copy-link" onclick="copyShareText()">📋</button>
  </div>
</div>

<!-- 放在 </body> 前 -->
<div class="share-toast" id="shareToast">Copied!</div>
```

## 4. JavaScript — 分享函数（每个游戏必须实现）

每个游戏只需要定义两个函数，其他都是通用的：

```javascript
// ===== 每个游戏自定义这两个 =====
function getShareText() {
  // 返回分享文案，包含成绩+挑战语气+链接
  return `🎮 [成绩描述]! Can you beat me?\n\nhttps://joy-grid.com/[游戏].html`;
}
function getShareUrl() {
  return 'https://joy-grid.com/[游戏].html';
}

// ===== 以下是通用代码，所有游戏一样 =====
function shareTwitter() {
  window.open('https://twitter.com/intent/tweet?text=' + encodeURIComponent(getShareText()), '_blank');
}
function shareFacebook() {
  window.open('https://www.facebook.com/sharer/sharer.php?u=' + encodeURIComponent(getShareUrl()), '_blank');
}
function shareWhatsApp() {
  window.open('https://wa.me/?text=' + encodeURIComponent(getShareText()), '_blank');
}
function nativeShare() {
  if (navigator.share) {
    navigator.share({title: 'Joy Grid', text: getShareText(), url: getShareUrl()}).catch(function(){});
  }
}
function copyShareText() {
  navigator.clipboard.writeText(getShareText()).then(function() {
    var t = document.getElementById('shareToast');
    t.classList.add('show');
    setTimeout(function() { t.classList.remove('show'); }, 2000);
  });
}
if (!navigator.share) {
  var ns = document.getElementById('nativeShareBtn');
  if (ns) ns.style.display = 'none';
}
```

## 5. OG 预览图规范

- 尺寸：1200 x 630 像素
- 格式：PNG（不要用 SVG）
- 命名：`og-[游戏名].png`
- 内容：左侧游戏预览 + 右侧标题和 Joy Grid logo
- 用 generate_og.py 脚本生成

## 6. Facebook 调试

新页面部署后，去 https://developers.facebook.com/tools/debug/ 
输入页面 URL 点 "Scrape Again"，强制 Facebook 刷新缓存。
