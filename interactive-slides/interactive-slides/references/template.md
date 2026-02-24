# Interactive Slides Template Reference

## 1. HTML Skeleton

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title><!-- PRESENTATION TITLE HERE --></title>
  <style>
    <!-- CSS GOES HERE (see section 2) -->
  </style>
</head>
<body>

<!-- GRID BACKGROUND -->
<div class="grid-bg"></div>

<!-- PRESENTATION CONTAINER -->
<div class="presentation" id="presentation">
  <!-- SLIDES GO HERE (see section 3) -->
</div>

<!-- LIGHTBOX FOR EXPANDED IMAGES -->
<div class="lightbox-overlay" id="lightbox" onclick="this.classList.remove('open')">
  <img id="lightboxImg" src="" alt="Diagram">
</div>

<!-- FULLSCREEN BUTTON -->
<button class="fullscreen-btn" id="fullscreenBtn" title="Fullscreen (F)">⛶</button>

<!-- PAGE DROPDOWN MENU -->
<div class="page-dropdown" id="pageDropdown"></div>

<!-- NAVIGATION BAR -->
<div class="nav-bar" id="navBar">
  <button class="nav-btn" id="homeBtn" title="Home (H)" style="font-size: 18px;">⌂</button>
  <button class="nav-btn" id="prevBtn" title="Previous (←)">‹</button>
  <div class="nav-progress" id="navProgress"></div>
  <button class="nav-btn" id="nextBtn" title="Next (→)">›</button>
  <div class="nav-counter" id="navCounter">1 / X</div>
  <button class="nav-btn" id="menuBtn" title="Slide Menu">☰</button>
</div>

<!-- JAVASCRIPT (see section 4) -->
<script>
  <!-- JAVASCRIPT CODE GOES HERE -->
</script>

</body>
</html>
```

---

## 2. CSS Styles

### 2.1 Root Variables & Theme Colors

```css
@import url('https://fonts.googleapis.com/css2?family=IBM+Plex+Sans+Thai:wght@300;400;500;600;700&family=Inter:wght@300;400;500;600;700;800;900&display=swap');

:root {
  /* === PRIMARY THEME COLORS === */
  /* REPLACE WITH YOUR BRAND COLORS */
  --primary: #126C62;              /* Main brand color */
  --primary-dark: #0E554D;         /* Darker shade */
  --primary-light: #1A8A7D;        /* Lighter shade */

  --secondary: #C69B57;            /* Secondary accent */
  --secondary-dark: #A8813E;
  --secondary-light: #D4AF6E;

  /* === ACCENT COLORS === */
  --accent-orange: #C69B57;
  --accent-red: #C0392B;
  --accent-purple: #6C5B7B;
  --accent-pink: #B5547A;
  --accent-amber: #D4A843;

  /* === BACKGROUND & CARD COLORS === */
  --bg-dark: #F5F2ED;              /* Main background */
  --bg-card: rgba(18, 108, 98, 0.05);
  --bg-card-hover: rgba(18, 108, 98, 0.1);

  /* === TEXT COLORS === */
  --text-primary: #1A2E2B;
  --text-secondary: #4A6B66;
  --text-muted: #7A9994;

  /* === BORDERS & GRID === */
  --grid-color: rgba(18, 108, 98, 0.06);
  --border-color: rgba(18, 108, 98, 0.12);

  /* === TRANSITION SPEEDS === */
  --transition-fast: 200ms;
  --transition-normal: 350ms;
  --transition-slow: 500ms;
}
```

### 2.2 Global Reset & Body

```css
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: 'IBM Plex Sans Thai', 'Inter', sans-serif;
  background: var(--bg-dark);
  color: var(--text-primary);
  overflow: hidden;
  width: 100vw;
  height: 100vh;
  cursor: default;
  -webkit-font-smoothing: antialiased;
}
```

### 2.3 Grid Background

```css
.grid-bg {
  position: fixed;
  inset: 0;
  background-image:
    linear-gradient(var(--grid-color) 1px, transparent 1px),
    linear-gradient(90deg, var(--grid-color) 1px, transparent 1px);
  background-size: 40px 40px;
  z-index: 0;
  pointer-events: none;
}

.grid-bg::after {
  content: '';
  position: absolute;
  inset: 0;
  background: radial-gradient(ellipse at 50% 0%, rgba(18,108,98,0.06) 0%, transparent 60%);
}
```

### 2.4 Slide Container

```css
.presentation {
  position: relative;
  width: 100vw;
  height: 100vh;
  overflow: hidden;
  z-index: 1;
}

.slide {
  position: absolute;
  inset: 0;
  display: flex;
  flex-direction: column;
  padding: clamp(24px, 3vw, 48px);
  opacity: 0;
  visibility: hidden;
  transform: translateX(60px);
  transition: opacity var(--transition-slow) ease,
              transform var(--transition-slow) cubic-bezier(0.34, 1.56, 0.64, 1),
              visibility 0s var(--transition-slow);
}

.slide.active {
  opacity: 1;
  visibility: visible;
  transform: translateX(0);
  transition-delay: 0s;
}

.slide.prev {
  transform: translateX(-60px);
}
```

### 2.5 Slide Header & Badge

```css
.slide-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-bottom: clamp(16px, 2vw, 32px);
  flex-shrink: 0;
}

.slide-badge {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  padding: 6px 14px;
  border-radius: 20px;
  font-size: clamp(10px, 1vw, 13px);
  font-weight: 600;
  text-transform: uppercase;
  letter-spacing: 0.05em;
  background: rgba(18, 108, 98, 0.1);
  color: var(--primary);
  border: 1px solid rgba(18, 108, 98, 0.2);
}

.slide-badge .dot {
  width: 6px;
  height: 6px;
  border-radius: 50%;
  background: var(--primary);
}

.slide-number {
  font-size: clamp(11px, 1vw, 14px);
  font-weight: 500;
  color: var(--text-muted);
  font-family: 'Inter', monospace;
}
```

### 2.6 Typography

```css
.slide-title {
  font-size: clamp(24px, 3.5vw, 56px);
  font-weight: 800;
  line-height: 1.15;
  margin-bottom: clamp(8px, 1vw, 16px);
  letter-spacing: -0.02em;
  background: linear-gradient(135deg, var(--text-primary) 0%, var(--primary) 100%);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

.slide-subtitle {
  font-size: clamp(13px, 1.4vw, 20px);
  color: var(--text-secondary);
  font-weight: 400;
  line-height: 1.5;
  max-width: 700px;
}

.slide-content {
  flex: 1;
  display: flex;
  flex-direction: column;
  justify-content: center;
  overflow: hidden;
}
```

### 2.7 Flow Container & Steps

```css
.flow-container {
  display: flex;
  flex-direction: column;
  gap: clamp(10px, 1.2vw, 18px);
  max-width: 1200px;
  margin: 0 auto;
  width: 100%;
}

.flow-row {
  display: flex;
  align-items: center;
  gap: clamp(6px, 0.8vw, 12px);
  flex-wrap: wrap;
  justify-content: center;
}

.flow-step {
  display: flex;
  align-items: center;
  gap: clamp(4px, 0.5vw, 8px);
  padding: clamp(8px, 0.8vw, 14px) clamp(12px, 1.2vw, 20px);
  border-radius: 10px;
  font-size: clamp(10px, 1.1vw, 15px);
  font-weight: 500;
  transition: all var(--transition-fast) ease;
  white-space: nowrap;
  border: 1px solid var(--border-color);
  background: var(--bg-card);
  backdrop-filter: blur(8px);
}

.flow-step:hover {
  transform: translateY(-2px);
  background: var(--bg-card-hover);
}

.flow-step .icon {
  font-size: clamp(14px, 1.4vw, 20px);
}

.flow-arrow {
  color: var(--text-muted);
  font-size: clamp(14px, 1.5vw, 22px);
  flex-shrink: 0;
}
```

### 2.8 Flow Step Highlight Colors

```css
.flow-step.highlight-blue {
  background: rgba(18, 108, 98, 0.08);
  border-color: rgba(18, 108, 98, 0.25);
  color: var(--primary);
}

.flow-step.highlight-green {
  background: rgba(18, 108, 98, 0.12);
  border-color: rgba(18, 108, 98, 0.35);
  color: #0E554D;
}

.flow-step.highlight-orange {
  background: rgba(198, 155, 87, 0.1);
  border-color: rgba(198, 155, 87, 0.3);
  color: #A8813E;
}

.flow-step.highlight-red {
  background: rgba(192, 57, 43, 0.08);
  border-color: rgba(192, 57, 43, 0.25);
  color: #C0392B;
}

.flow-step.highlight-purple {
  background: rgba(108, 91, 123, 0.1);
  border-color: rgba(108, 91, 123, 0.25);
  color: #6C5B7B;
}

.flow-step.highlight-amber {
  background: rgba(212, 168, 67, 0.1);
  border-color: rgba(212, 168, 67, 0.25);
  color: #9A7B2F;
}
```

### 2.9 Cards Grid

```css
.cards-grid {
  display: grid;
  gap: clamp(10px, 1.2vw, 18px);
  max-width: 1200px;
  margin: 0 auto;
  width: 100%;
}

.cards-grid.cols-2 {
  grid-template-columns: repeat(2, 1fr);
}

.cards-grid.cols-3 {
  grid-template-columns: repeat(3, 1fr);
}

.cards-grid.cols-4 {
  grid-template-columns: repeat(4, 1fr);
}

.card {
  padding: clamp(14px, 1.5vw, 24px);
  border-radius: 14px;
  border: 1px solid var(--border-color);
  background: var(--bg-card);
  backdrop-filter: blur(8px);
  transition: all var(--transition-fast) ease;
}

.card:hover {
  transform: translateY(-3px);
  background: var(--bg-card-hover);
  border-color: rgba(18, 108, 98, 0.25);
}

.card-icon {
  font-size: clamp(24px, 2.5vw, 40px);
  margin-bottom: clamp(6px, 0.8vw, 12px);
}

.card-title {
  font-size: clamp(13px, 1.3vw, 18px);
  font-weight: 700;
  margin-bottom: clamp(4px, 0.5vw, 8px);
  color: var(--text-primary);
}

.card-desc {
  font-size: clamp(10px, 1vw, 14px);
  color: var(--text-secondary);
  line-height: 1.5;
}

.card-tag {
  display: inline-block;
  padding: 3px 10px;
  border-radius: 12px;
  font-size: clamp(9px, 0.8vw, 12px);
  font-weight: 600;
  margin-top: 8px;
}

.tag-blue {
  background: rgba(18,108,98,0.1);
  color: var(--primary);
}

.tag-green {
  background: rgba(18,108,98,0.15);
  color: #0E554D;
}

.tag-orange {
  background: rgba(198,155,87,0.15);
  color: #A8813E;
}

.tag-red {
  background: rgba(192,57,43,0.12);
  color: #C0392B;
}

.tag-purple {
  background: rgba(108,91,123,0.12);
  color: #6C5B7B;
}
```

### 2.10 State Machine

```css
.state-machine {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: clamp(4px, 0.6vw, 10px);
  flex-wrap: wrap;
  margin: clamp(8px, 1vw, 16px) 0;
}

.state-node {
  padding: clamp(8px, 0.8vw, 14px) clamp(14px, 1.4vw, 24px);
  border-radius: 10px;
  font-size: clamp(10px, 1vw, 14px);
  font-weight: 600;
  text-align: center;
  border: 1px solid var(--border-color);
  background: var(--bg-card);
  min-width: clamp(80px, 8vw, 130px);
}

.state-arrow {
  color: var(--text-muted);
  font-size: clamp(16px, 1.6vw, 24px);
}
```

### 2.11 Decision Tree

```css
.decision-tree {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: clamp(6px, 0.8vw, 12px);
}

.decision-node {
  padding: clamp(10px, 1vw, 16px) clamp(16px, 1.6vw, 28px);
  border-radius: 12px;
  font-size: clamp(11px, 1.1vw, 15px);
  font-weight: 600;
  text-align: center;
  max-width: 500px;
}

.decision-diamond {
  background: rgba(198, 155, 87, 0.1);
  border: 1px solid rgba(198, 155, 87, 0.3);
  color: #9A7B2F;
  transform: rotate(0deg);
  position: relative;
}

.decision-diamond::before {
  content: '◆';
  margin-right: 8px;
  opacity: 0.6;
}

.decision-branches {
  display: flex;
  gap: clamp(16px, 2vw, 32px);
  justify-content: center;
}

.branch {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 8px;
}

.branch-label {
  font-size: clamp(10px, 0.9vw, 13px);
  font-weight: 700;
  padding: 3px 12px;
  border-radius: 8px;
}

.branch-yes {
  background: rgba(18,108,98,0.12);
  color: #0E554D;
}

.branch-no {
  background: rgba(192,57,43,0.1);
  color: #C0392B;
}

.branch-box {
  padding: clamp(8px, 0.8vw, 14px) clamp(14px, 1.4vw, 24px);
  border-radius: 10px;
  font-size: clamp(10px, 1vw, 14px);
  background: var(--bg-card);
  border: 1px solid var(--border-color);
  text-align: center;
  color: var(--text-secondary);
}
```

### 2.12 Mini Cards & Split Row

```css
.split-row {
  display: flex;
  gap: clamp(8px, 1vw, 16px);
  justify-content: center;
  flex-wrap: wrap;
}

.mini-card {
  flex: 1;
  min-width: 200px;
  max-width: 350px;
  padding: clamp(12px, 1.2vw, 20px);
  border-radius: 12px;
  border: 1px solid var(--border-color);
  background: var(--bg-card);
}

.mini-card-title {
  font-size: clamp(12px, 1.2vw, 16px);
  font-weight: 700;
  margin-bottom: 8px;
  display: flex;
  align-items: center;
  gap: 8px;
}

.mini-card ul {
  list-style: none;
  display: flex;
  flex-direction: column;
  gap: 4px;
}

.mini-card ul li {
  font-size: clamp(10px, 0.95vw, 13px);
  color: var(--text-secondary);
  padding-left: 14px;
  position: relative;
}

.mini-card ul li::before {
  content: '›';
  position: absolute;
  left: 0;
  color: var(--primary);
  font-weight: 700;
}
```

### 2.13 Two Column Layout

```css
.two-col {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: clamp(16px, 2vw, 32px);
  max-width: 1200px;
  margin: 0 auto;
  width: 100%;
}

.col-section {
  display: flex;
  flex-direction: column;
  gap: clamp(8px, 1vw, 14px);
}
```

### 2.14 Section Label & Bullet List

```css
.section-label {
  font-size: clamp(10px, 0.9vw, 13px);
  font-weight: 600;
  text-transform: uppercase;
  letter-spacing: 0.1em;
  color: var(--text-muted);
  margin-bottom: clamp(8px, 1vw, 14px);
  padding-bottom: 8px;
  border-bottom: 1px solid var(--border-color);
}

.bullet-list {
  list-style: none;
  display: flex;
  flex-direction: column;
  gap: clamp(6px, 0.7vw, 10px);
}

.bullet-list li {
  display: flex;
  align-items: flex-start;
  gap: clamp(8px, 0.8vw, 12px);
  font-size: clamp(11px, 1.1vw, 15px);
  color: var(--text-secondary);
  line-height: 1.5;
}

.bullet-list li .bullet {
  width: 6px;
  height: 6px;
  border-radius: 50%;
  background: var(--primary);
  flex-shrink: 0;
  margin-top: 7px;
}
```

### 2.15 Cover Slide

```css
.cover-content {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  text-align: center;
  gap: clamp(12px, 1.5vw, 24px);
}

.cover-logo {
  font-size: clamp(60px, 8vw, 120px);
  line-height: 1;
  margin-bottom: clamp(8px, 1vw, 16px);
}

.cover-title {
  font-size: clamp(32px, 5vw, 72px);
  font-weight: 900;
  letter-spacing: -0.03em;
  background: linear-gradient(135deg, var(--primary) 0%, var(--primary-light) 50%, var(--secondary) 100%);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
  animation: shimmer 4s ease infinite;
  background-size: 200% 200%;
}

.cover-subtitle {
  font-size: clamp(14px, 1.8vw, 26px);
  color: var(--text-secondary);
  font-weight: 400;
  max-width: 600px;
}

.cover-roles {
  display: flex;
  gap: clamp(12px, 1.5vw, 24px);
  margin-top: clamp(12px, 1.5vw, 24px);
}

.role-chip {
  padding: clamp(8px, 0.8vw, 12px) clamp(16px, 1.6vw, 28px);
  border-radius: 50px;
  font-size: clamp(11px, 1.1vw, 15px);
  font-weight: 600;
  border: 1px solid var(--border-color);
  background: var(--bg-card);
  transition: all var(--transition-fast) ease;
}

.role-chip:hover {
  transform: translateY(-2px) scale(1.05);
}
```

### 2.16 End Slide

```css
.end-content {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  text-align: center;
  gap: 20px;
}

.end-title {
  font-size: clamp(36px, 5vw, 80px);
  font-weight: 900;
  background: linear-gradient(135deg, var(--primary), var(--secondary), var(--primary-light));
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
  animation: shimmer 3s ease infinite;
  background-size: 200% 200%;
}
```

### 2.17 Vertical Flow

```css
.vertical-flow {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: clamp(4px, 0.5vw, 8px);
}

.v-arrow {
  color: var(--text-muted);
  font-size: clamp(14px, 1.4vw, 20px);
}
```

### 2.18 Navigation Bar

```css
.nav-bar {
  position: fixed;
  bottom: clamp(16px, 2vw, 28px);
  left: 50%;
  transform: translateX(-50%);
  display: flex;
  align-items: center;
  gap: 12px;
  padding: 8px 16px;
  border-radius: 16px;
  background: rgba(255, 255, 255, 0.85);
  backdrop-filter: blur(16px);
  border: 1px solid rgba(18,108,98,0.12);
  box-shadow: 0 4px 24px rgba(0,0,0,0.08);
  z-index: 100;
  transition: opacity var(--transition-fast) ease;
}

.nav-btn {
  width: 40px;
  height: 40px;
  border-radius: 10px;
  border: 1px solid var(--border-color);
  background: transparent;
  color: var(--text-secondary);
  font-size: 16px;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all var(--transition-fast) ease;
}

.nav-btn:hover {
  background: rgba(18,108,98,0.08);
  color: var(--primary);
  border-color: rgba(18,108,98,0.3);
}

.nav-btn:active {
  transform: scale(0.92);
}

.nav-btn:disabled {
  opacity: 0.3;
  cursor: not-allowed;
}
```

### 2.19 Navigation Progress Dots

```css
.nav-progress {
  display: flex;
  align-items: center;
  gap: 4px;
}

.nav-dot {
  width: 8px;
  height: 8px;
  border-radius: 50%;
  background: var(--text-muted);
  opacity: 0.3;
  transition: all var(--transition-fast) ease;
  cursor: pointer;
}

.nav-dot.active {
  background: var(--primary);
  opacity: 1;
  width: 22px;
  border-radius: 4px;
}

.nav-dot:hover {
  opacity: 0.7;
}

.nav-counter {
  font-size: 13px;
  font-weight: 600;
  color: var(--text-muted);
  font-family: 'Inter', monospace;
  min-width: 50px;
  text-align: center;
}
```

### 2.20 Page Dropdown Menu

```css
.page-dropdown {
  position: fixed;
  bottom: clamp(72px, 8vw, 96px);
  left: 50%;
  transform: translateX(-50%) translateY(10px);
  background: rgba(255, 255, 255, 0.95);
  backdrop-filter: blur(16px);
  box-shadow: 0 4px 24px rgba(0,0,0,0.1);
  border: 1px solid var(--border-color);
  border-radius: 14px;
  padding: 8px;
  z-index: 101;
  max-height: 60vh;
  overflow-y: auto;
  min-width: 280px;
  opacity: 0;
  visibility: hidden;
  transition: all var(--transition-fast) ease;
}

.page-dropdown.open {
  opacity: 1;
  visibility: visible;
  transform: translateX(-50%) translateY(0);
}

.page-dropdown-item {
  display: flex;
  align-items: center;
  gap: 10px;
  padding: 8px 14px;
  border-radius: 8px;
  cursor: pointer;
  font-size: 13px;
  color: var(--text-secondary);
  transition: all var(--transition-fast) ease;
}

.page-dropdown-item:hover {
  background: var(--bg-card-hover);
  color: var(--text-primary);
}

.page-dropdown-item.active {
  background: rgba(18, 108, 98, 0.1);
  color: var(--primary);
  font-weight: 600;
}

.page-dropdown-item .item-num {
  font-weight: 700;
  font-family: 'Inter', monospace;
  font-size: 11px;
  min-width: 24px;
  text-align: center;
  padding: 2px 6px;
  border-radius: 6px;
  background: rgba(18,108,98,0.06);
}
```

### 2.21 Fullscreen Button

```css
.fullscreen-btn {
  position: fixed;
  top: 16px;
  right: 16px;
  z-index: 100;
  width: 36px;
  height: 36px;
  border-radius: 8px;
  border: 1px solid var(--border-color);
  background: rgba(255, 255, 255, 0.8);
  backdrop-filter: blur(8px);
  box-shadow: 0 2px 8px rgba(0,0,0,0.06);
  color: var(--text-muted);
  font-size: 14px;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all var(--transition-fast) ease;
}

.fullscreen-btn:hover {
  color: var(--primary);
  border-color: rgba(18,108,98,0.3);
}
```

### 2.22 Image Styles

```css
.slide-illust {
  position: absolute;
  right: clamp(24px, 4vw, 60px);
  top: clamp(60px, 8vh, 100px);
  opacity: 0.18;
  pointer-events: none;
  z-index: 0;
}

.slide-illust img {
  max-height: clamp(120px, 18vh, 220px);
  max-width: clamp(120px, 18vh, 220px);
  object-fit: contain;
}

.slide.active .slide-illust {
  animation: fadeInUp 0.8s 0.3s ease forwards;
  opacity: 0;
}

.slide-phone {
  display: flex;
  justify-content: center;
  gap: clamp(8px, 1vw, 16px);
  margin: clamp(4px, 0.5vw, 8px) 0;
}

.slide-phone img {
  max-height: clamp(180px, 28vh, 320px);
  object-fit: contain;
  border-radius: 16px;
  box-shadow: 0 4px 20px rgba(0,0,0,0.08);
  cursor: pointer;
  transition: all var(--transition-fast) ease;
}

.slide-phone img:hover {
  transform: scale(1.03);
  box-shadow: 0 8px 32px rgba(0,0,0,0.12);
}

.cover-app-img {
  max-height: clamp(120px, 16vh, 200px);
  object-fit: contain;
  border-radius: 20px;
  box-shadow: 0 8px 32px rgba(0,0,0,0.12);
}
```

### 2.23 Lightbox Overlay

```css
.lightbox-overlay {
  position: fixed;
  inset: 0;
  background: rgba(0,0,0,0.8);
  z-index: 200;
  display: flex;
  align-items: center;
  justify-content: center;
  opacity: 0;
  visibility: hidden;
  transition: all 0.3s ease;
  cursor: zoom-out;
}

.lightbox-overlay.open {
  opacity: 1;
  visibility: visible;
}

.lightbox-overlay img {
  max-width: 92vw;
  max-height: 90vh;
  object-fit: contain;
  border-radius: 12px;
  box-shadow: 0 8px 40px rgba(0,0,0,0.3);
}
```

### 2.24 Highlight Tags

```css
.hl {
  font-weight: 700;
}

.hl-blue {
  color: var(--primary);
}

.hl-green {
  color: #0E554D;
}

.hl-orange {
  color: #A8813E;
}

.hl-red {
  color: #C0392B;
}

.hl-purple {
  color: #6C5B7B;
}
```

### 2.25 Animations

```css
@keyframes shimmer {
  0%, 100% { background-position: 0% 50%; }
  50% { background-position: 100% 50%; }
}

@keyframes fadeInUp {
  from { opacity: 0; transform: translateY(20px); }
  to { opacity: 1; transform: translateY(0); }
}

.slide.active .anim-item {
  animation: fadeInUp 0.5s ease forwards;
  opacity: 0;
}

.slide.active .anim-item:nth-child(1) { animation-delay: 0.1s; }
.slide.active .anim-item:nth-child(2) { animation-delay: 0.2s; }
.slide.active .anim-item:nth-child(3) { animation-delay: 0.3s; }
.slide.active .anim-item:nth-child(4) { animation-delay: 0.4s; }
.slide.active .anim-item:nth-child(5) { animation-delay: 0.5s; }
.slide.active .anim-item:nth-child(6) { animation-delay: 0.6s; }
.slide.active .anim-item:nth-child(7) { animation-delay: 0.7s; }
.slide.active .anim-item:nth-child(8) { animation-delay: 0.8s; }
```

### 2.26 Scrollbar

```css
::-webkit-scrollbar {
  width: 4px;
}

::-webkit-scrollbar-track {
  background: transparent;
}

::-webkit-scrollbar-thumb {
  background: var(--text-muted);
  border-radius: 4px;
}
```

---

## 3. Slide HTML Examples

### 3.1 Cover Slide

```html
<div class="slide active" data-slide="0">
  <div class="slide-content cover-content">
    <div class="cover-title"><!-- TITLE HERE --></div>
    <div class="cover-subtitle"><!-- SUBTITLE HERE --></div>
    <div class="cover-roles">
      <div class="role-chip anim-item" style="border-color: rgba(18,108,98,0.3); color: var(--primary);"><!-- ROLE 1 --></div>
      <div class="role-chip anim-item" style="border-color: rgba(18,108,98,0.25); color: #0E554D;"><!-- ROLE 2 --></div>
      <div class="role-chip anim-item" style="border-color: rgba(108,91,123,0.3); color: #6C5B7B;"><!-- ROLE 3 --></div>
      <div class="role-chip anim-item" style="border-color: rgba(198,155,87,0.3); color: #A8813E;"><!-- ROLE 4 --></div>
    </div>
  </div>
</div>
```

### 3.2 Standard Slide with Header

```html
<div class="slide" data-slide="1">
  <div class="slide-header">
    <div class="slide-badge"><span class="dot"></span><!-- CATEGORY --></div>
    <div class="slide-number">01 / XX</div>
  </div>
  <div class="slide-title"><!-- TITLE HERE --></div>
  <div class="slide-subtitle"><!-- SUBTITLE HERE --></div>
  <div class="slide-content">
    <!-- CONTENT HERE -->
  </div>
</div>
```

### 3.3 Flow Slide

```html
<div class="slide" data-slide="2">
  <div class="slide-header">
    <div class="slide-badge"><span class="dot"></span>Flow</div>
    <div class="slide-number">02 / XX</div>
  </div>
  <div class="slide-title"><!-- TITLE --></div>
  <div class="slide-subtitle"><!-- SUBTITLE --></div>
  <div class="slide-content">
    <div class="flow-container">
      <!-- ROW 1 -->
      <div class="flow-row anim-item">
        <div class="flow-step highlight-blue"><span class="icon">🔗</span> Step 1</div>
        <div class="flow-arrow">→</div>
        <div class="flow-step highlight-blue"><span class="icon">🛒</span> Step 2</div>
        <div class="flow-arrow">→</div>
        <div class="flow-step highlight-orange"><span class="icon">📝</span> Step 3</div>
      </div>

      <!-- ROW 2 -->
      <div class="flow-row anim-item">
        <div class="flow-step highlight-green"><span class="icon">💳</span> Step 4</div>
        <div class="flow-arrow">→</div>
        <div class="flow-step highlight-green"><span class="icon">✅</span> Step 5</div>
      </div>
    </div>
  </div>
</div>
```

### 3.4 Cards Grid Slide (4 columns)

```html
<div class="slide" data-slide="3">
  <div class="slide-header">
    <div class="slide-badge"><span class="dot"></span>Overview</div>
    <div class="slide-number">03 / XX</div>
  </div>
  <div class="slide-title"><!-- TITLE --></div>
  <div class="slide-subtitle"><!-- SUBTITLE --></div>
  <div class="slide-content">
    <div class="cards-grid cols-4" style="margin-top: 24px;">
      <div class="card anim-item">
        <div class="card-icon">🎯</div>
        <div class="card-title">Card Title 1</div>
        <div class="card-desc">Card description goes here</div>
        <div class="card-tag tag-blue">Tag 1</div>
      </div>
      <div class="card anim-item">
        <div class="card-icon">🎯</div>
        <div class="card-title">Card Title 2</div>
        <div class="card-desc">Card description goes here</div>
        <div class="card-tag tag-green">Tag 2</div>
      </div>
      <!-- MORE CARDS -->
    </div>
  </div>
</div>
```

### 3.5 State Machine Slide

```html
<div class="slide" data-slide="4">
  <div class="slide-header">
    <div class="slide-badge"><span class="dot"></span>Status</div>
    <div class="slide-number">04 / XX</div>
  </div>
  <div class="slide-title"><!-- TITLE --></div>
  <div class="slide-subtitle"><!-- SUBTITLE --></div>
  <div class="slide-content">
    <div class="state-machine anim-item">
      <div class="state-node highlight-blue">💳 State 1</div>
      <div class="state-arrow">→</div>
      <div class="state-node highlight-blue">📝 State 2</div>
      <div class="state-arrow">→</div>
      <div class="state-node highlight-amber">📦 State 3</div>
      <div class="state-arrow">→</div>
      <div class="state-node highlight-green">✅ State 4</div>
    </div>
  </div>
</div>
```

### 3.6 Decision Tree Slide

```html
<div class="slide" data-slide="5">
  <div class="slide-header">
    <div class="slide-badge"><span class="dot"></span>Logic</div>
    <div class="slide-number">05 / XX</div>
  </div>
  <div class="slide-title"><!-- TITLE --></div>
  <div class="slide-subtitle"><!-- SUBTITLE --></div>
  <div class="slide-content">
    <div class="decision-tree anim-item">
      <div class="decision-node decision-diamond">Decision Question?</div>
      <div class="decision-branches" style="margin-top: 12px;">
        <div class="branch">
          <div class="branch-label branch-yes">Yes</div>
          <div class="branch-box">Outcome A</div>
        </div>
        <div class="branch">
          <div class="branch-label branch-no">No</div>
          <div class="branch-box">Outcome B</div>
        </div>
      </div>
    </div>
  </div>
</div>
```

### 3.7 Split Row with Mini Cards

```html
<div class="slide" data-slide="6">
  <div class="slide-header">
    <div class="slide-badge"><span class="dot"></span>Details</div>
    <div class="slide-number">06 / XX</div>
  </div>
  <div class="slide-title"><!-- TITLE --></div>
  <div class="slide-subtitle"><!-- SUBTITLE --></div>
  <div class="slide-content">
    <div class="section-label anim-item">SECTION NAME</div>
    <div class="split-row anim-item">
      <div class="mini-card">
        <div class="mini-card-title">📋 Card 1</div>
        <ul>
          <li>Bullet point 1</li>
          <li>Bullet point 2</li>
        </ul>
      </div>
      <div class="mini-card">
        <div class="mini-card-title">📊 Card 2</div>
        <ul>
          <li>Bullet point 1</li>
          <li>Bullet point 2</li>
        </ul>
      </div>
      <div class="mini-card">
        <div class="mini-card-title">🎯 Card 3</div>
        <ul>
          <li>Bullet point 1</li>
          <li>Bullet point 2</li>
        </ul>
      </div>
    </div>
  </div>
</div>
```

### 3.8 Two Column Layout

```html
<div class="slide" data-slide="7">
  <div class="slide-header">
    <div class="slide-badge"><span class="dot"></span>Comparison</div>
    <div class="slide-number">07 / XX</div>
  </div>
  <div class="slide-title"><!-- TITLE --></div>
  <div class="slide-subtitle"><!-- SUBTITLE --></div>
  <div class="slide-content">
    <div class="two-col">
      <div class="col-section anim-item">
        <h3 style="font-weight: 700; margin-bottom: 12px;">Left Column Title</h3>
        <ul class="bullet-list">
          <li><span class="bullet"></span> Point 1</li>
          <li><span class="bullet"></span> Point 2</li>
          <li><span class="bullet"></span> Point 3</li>
        </ul>
      </div>
      <div class="col-section anim-item">
        <h3 style="font-weight: 700; margin-bottom: 12px;">Right Column Title</h3>
        <ul class="bullet-list">
          <li><span class="bullet"></span> Point A</li>
          <li><span class="bullet"></span> Point B</li>
          <li><span class="bullet"></span> Point C</li>
        </ul>
      </div>
    </div>
  </div>
</div>
```

### 3.9 End/Thank You Slide

```html
<div class="slide" data-slide="15">
  <div class="slide-content end-content">
    <img src="images/Success.png" alt="Success" style="max-height: clamp(100px, 14vh, 180px); object-fit: contain;">
    <div class="end-title"><!-- THANK YOU TITLE --></div>
    <div class="slide-subtitle" style="max-width: 500px;"><!-- CLOSING MESSAGE --></div>
    <div class="cover-roles" style="margin-top: 24px;">
      <div class="role-chip"><!-- FINAL ITEM 1 --></div>
      <div class="role-chip"><!-- FINAL ITEM 2 --></div>
    </div>
  </div>
</div>
```

---

## 4. Navigation JavaScript

```javascript
// Lightbox function (global scope)
function openLightbox(src) {
  const lb = document.getElementById('lightbox');
  document.getElementById('lightboxImg').src = src;
  lb.classList.add('open');
}

(function() {
  const slides = document.querySelectorAll('.slide');
  const totalSlides = slides.length;
  let currentSlide = 0;
  let animTimer = null;
  let dropdownOpen = false;

  // CONFIGURE: Update slide titles array with your slide titles
  const slideTitles = [
    'Cover — Title',
    'Slide 1 Title',
    'Slide 2 Title',
    'Slide 3 Title',
    'Slide 4 Title',
    // ... add more slide titles
  ];

  // Build navigation dots
  const navProgress = document.getElementById('navProgress');
  for (let i = 0; i < totalSlides; i++) {
    const dot = document.createElement('div');
    dot.className = 'nav-dot' + (i === 0 ? ' active' : '');
    dot.addEventListener('click', () => goToSlide(i));
    navProgress.appendChild(dot);
  }

  // Build dropdown menu
  const dropdown = document.getElementById('pageDropdown');
  slideTitles.forEach((title, i) => {
    const item = document.createElement('div');
    item.className = 'page-dropdown-item' + (i === 0 ? ' active' : '');
    item.innerHTML = `<span class="item-num">${String(i).padStart(2, '0')}</span> ${title}`;
    item.addEventListener('click', (e) => {
      e.stopPropagation();
      const targetIndex = i;
      toggleDropdown(false);
      requestAnimationFrame(() => goToSlide(targetIndex));
    });
    dropdown.appendChild(item);
  });

  // Main navigation function
  function goToSlide(index) {
    if (index === currentSlide || index < 0 || index >= totalSlides) return;

    // Cancel any ongoing animation
    if (animTimer) {
      clearTimeout(animTimer);
      slides.forEach(s => {
        s.classList.remove('active', 'prev');
        s.style.transform = '';
      });
    }

    const prev = slides[currentSlide];
    const next = slides[index];
    const goingForward = index > currentSlide;

    // Remove active from previous slide
    prev.classList.remove('active');
    if (goingForward) {
      prev.classList.add('prev');
    }

    // Set up incoming slide position
    next.style.transition = 'none';
    next.style.transform = goingForward ? 'translateX(60px)' : 'translateX(-60px)';
    next.style.opacity = '0';
    next.offsetHeight; // force reflow

    // Enable transitions and animate in
    next.style.transition = '';
    next.style.transform = '';
    next.style.opacity = '';
    next.classList.add('active');

    currentSlide = index;
    updateUI();

    // Clean up animation
    animTimer = setTimeout(() => {
      prev.classList.remove('prev');
      prev.style.transform = '';
      animTimer = null;
    }, 500);
  }

  // Update UI elements (counter, dots, button states)
  function updateUI() {
    // Counter
    document.getElementById('navCounter').textContent = `${currentSlide + 1} / ${totalSlides}`;

    // Dots
    const dots = navProgress.querySelectorAll('.nav-dot');
    dots.forEach((d, i) => d.classList.toggle('active', i === currentSlide));

    // Buttons
    document.getElementById('prevBtn').disabled = currentSlide === 0;
    document.getElementById('nextBtn').disabled = currentSlide === totalSlides - 1;
    document.getElementById('homeBtn').disabled = currentSlide === 0;

    // Dropdown items
    const items = dropdown.querySelectorAll('.page-dropdown-item');
    items.forEach((item, i) => item.classList.toggle('active', i === currentSlide));
  }

  // Toggle dropdown visibility
  function toggleDropdown(state) {
    dropdownOpen = state !== undefined ? state : !dropdownOpen;
    dropdown.classList.toggle('open', dropdownOpen);
  }

  // Navigation button event listeners
  document.getElementById('homeBtn').addEventListener('click', () => goToSlide(0));
  document.getElementById('prevBtn').addEventListener('click', () => goToSlide(currentSlide - 1));
  document.getElementById('nextBtn').addEventListener('click', () => goToSlide(currentSlide + 1));
  document.getElementById('menuBtn').addEventListener('click', (e) => {
    e.stopPropagation();
    toggleDropdown();
  });

  // Fullscreen
  document.getElementById('fullscreenBtn').addEventListener('click', () => {
    if (!document.fullscreenElement) {
      document.documentElement.requestFullscreen();
    } else {
      document.exitFullscreen();
    }
  });

  // Keyboard navigation
  document.addEventListener('keydown', (e) => {
    if (e.key === 'ArrowRight' || e.key === ' ') {
      e.preventDefault();
      goToSlide(currentSlide + 1);
    } else if (e.key === 'ArrowLeft') {
      e.preventDefault();
      goToSlide(currentSlide - 1);
    } else if (e.key === 'Home' || e.key === 'h' || e.key === 'H') {
      e.preventDefault();
      goToSlide(0);
    } else if (e.key === 'End') {
      e.preventDefault();
      goToSlide(totalSlides - 1);
    } else if (e.key === 'f' || e.key === 'F') {
      document.getElementById('fullscreenBtn').click();
    } else if (e.key === 'Escape') {
      toggleDropdown(false);
      document.getElementById('lightbox').classList.remove('open');
    }
  });

  // Touch swipe support
  let touchStartX = 0;
  document.addEventListener('touchstart', (e) => {
    touchStartX = e.touches[0].clientX;
  });
  document.addEventListener('touchend', (e) => {
    const diff = touchStartX - e.changedTouches[0].clientX;
    if (Math.abs(diff) > 50) {
      diff > 0 ? goToSlide(currentSlide + 1) : goToSlide(currentSlide - 1);
    }
  });

  // Close dropdown on outside click
  document.addEventListener('click', (e) => {
    if (dropdownOpen && !dropdown.contains(e.target) && e.target.id !== 'menuBtn') {
      toggleDropdown(false);
    }
  });

  // Initialize UI
  updateUI();
})();
```

---

## Usage Notes

1. **Color Theme**: All colors are defined in `:root` variables. Update the primary, secondary, and accent colors to match your brand.

2. **Fonts**: Currently using IBM Plex Sans Thai and Inter. Change the `@import` line if you prefer different fonts.

3. **Responsive Design**: All sizes use `clamp()` for responsive scaling across different screen sizes.

4. **Animation Items**: Add the `anim-item` class to elements that should animate on slide entry.

5. **Slide Titles**: Update the `slideTitles` array in the JavaScript with your actual slide titles.

6. **Images**: Use the `openLightbox(src)` function to enable zoom-in lightbox effect on images.

7. **Custom Classes**: Use the highlight color classes (`highlight-blue`, `highlight-green`, etc.) for consistent theming across flow steps and cards.

8. **Keyboard Shortcuts**:
   - Arrow Left/Right: Navigate slides
   - Space: Next slide
   - Home: First slide
   - End: Last slide
   - F: Fullscreen
   - Esc: Close menus/lightbox

9. **Touch Support**: Swipe left/right to navigate (50px threshold).

10. **Accessibility**: All buttons have title attributes for tooltips and keyboard support.
