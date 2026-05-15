# 政府文书风格主题 实施计划

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** 把申根签证工具的视觉风格从产品页风格切换到政府文书数字化助手风格，且保持 fork 同步上游零冲突契约。

**Architecture:** 新增独立 `theme.css`，通过同选择器后置定义自然覆盖 upstream `<style>` 块；`index.html` 仅追加 1 行 `<link>` 注入。不改业务逻辑、不改 HTML 结构、不引入构建工具或新依赖。

**Tech Stack:** 纯 CSS（无预处理器）、原生 HTML。无 JS 改动、无测试框架，验收用 Chrome DevTools 多视口手动检查。

**Spec:** `docs/superpowers/specs/2026-05-15-government-theme-design.md`

---

## File Structure

| 操作 | 路径 | 责任 |
|------|------|------|
| 创建 | `theme.css` | 全部主题代码，~150 行，按 12 段组织 |
| 修改 | `index.html` | 仅 1 行：在 `</head>` 前加 `<link rel="stylesheet" href="theme.css">` |
| 创建 | `docs/superpowers/plans/2026-05-15-government-theme-implementation.md` | 本计划 |

---

## 验证入口（每个 task 通用）

每次改动 `theme.css` 后，按以下方式验证：

```bash
# macOS：打开浏览器
open /Users/uc/Workshop/privte/schengen-visa-tool/index.html
```

或在已打开页面 **强制刷新**（避免缓存）：

- macOS Chrome: `⌘ + Shift + R`
- macOS Safari: `⌥ + ⌘ + R`

DevTools 关键开关：

- 切换视口：`⌘ + Shift + M` → 选 iPhone SE / iPad / Responsive
- 模拟 reduced motion：DevTools → ⋮ → More tools → Rendering → "Emulate CSS media feature prefers-reduced-motion"
- 验证元素尺寸：选中元素 → Computed 标签查看 padding/font-size

---

## Tasks

---

### Task 1: 创建 theme.css 骨架 + 注入 link

**Files:**
- Create: `theme.css`
- Modify: `index.html` 行 91-92 之间

- [ ] **Step 1: 创建 `theme.css` 骨架（占位 12 段）**

写入 `/Users/uc/Workshop/privte/schengen-visa-tool/theme.css`：

```css
/* ============================================================
   Schengen Visa Tool · Government Document Theme
   Fork-side override of upstream/main styles.
   Do NOT edit upstream <style> block in index.html;
   this file owns the theme.
   ============================================================ */

/* === 1. Base & Typography ============================ */

/* === 2. Header ======================================= */

/* === 3. Top Storage Toolbar (override inline style) === */

/* === 4. Progress Bar ================================= */

/* === 5. Cards & Sections ============================= */

/* === 6. Form Fields ================================== */

/* === 7. Radios / Checks ============================== */

/* === 8. Buttons ====================================== */

/* === 9. Country Cards ================================ */

/* === 10. Sumbox / Tip ================================ */

/* === 11. Responsive ================================== */

/* === 12. Reduced Motion ============================== */
```

- [ ] **Step 2: 在 `index.html` 注入 link**

修改 `index.html`，在 `</style>`（行 91）后、`</head>`（行 92）前插入一行 `<link rel="stylesheet" href="theme.css">`。

修改前（行 91-92）：

```html
@media(max-width:580px){.g2,.g3{grid-template-columns:1fr}.sumgrid{grid-template-columns:1fr}}</style>
</head>
```

修改后：

```html
@media(max-width:580px){.g2,.g3{grid-template-columns:1fr}.sumgrid{grid-template-columns:1fr}}</style>
<link rel="stylesheet" href="theme.css">
</head>
```

- [ ] **Step 3: 浏览器验证 link 已加载**

```bash
open /Users/uc/Workshop/privte/schengen-visa-tool/index.html
```

DevTools → Network 标签 → 刷新 → 应能看到 `theme.css` 200 OK。
DevTools → Sources 标签 → 应能在文件树看到 `theme.css`。
**预期：** 视觉无任何变化（骨架是空的），但文件已加载。

- [ ] **Step 4: Commit**

```bash
git add theme.css index.html
git commit -m "feat: 添加 fork 侧专属 theme.css 骨架并注入 link [fork-only]"
```

---

### Task 2: § 1 Base & Typography + § 2 Header

**Files:**
- Modify: `theme.css` 第 1、2 段

- [ ] **Step 1: 写入 § 1 Base & Typography**

替换 `theme.css` 中 `/* === 1. Base & Typography ============================ */` 段，整段替换为：

```css
/* === 1. Base & Typography ============================ */
body {
  background: #F8FAFC;
  color: #0F172A;
  font-size: 16px;
  line-height: 1.5;
  font-family: 'PingFang SC', 'Microsoft YaHei', system-ui, -apple-system, sans-serif;
}
h1, .ctitle { line-height: 1.3; }
button { line-height: 1; }
```

- [ ] **Step 2: 写入 § 2 Header**

替换 `/* === 2. Header ======================================= */` 段：

```css
/* === 2. Header ======================================= */
.hdr {
  background: #1E3A5F;
  padding: 20px 24px;
  box-shadow: 0 1px 3px rgba(15,23,42,.08);
}
.hdr h1 {
  font-size: 20px;
  line-height: 1.3;
}
.hdr p {
  font-size: 13px;
  opacity: .85;
}
```

- [ ] **Step 3: 浏览器验证**

刷新 `index.html`（`⌘ + Shift + R`），检查：

| 检查项 | 预期 |
|--------|------|
| body 背景 | 改为 `#F8FAFC` 纸白（原本是 `#f0f4f8` 偏蓝灰） |
| Header 渐变 | **消失**，变为纯深海军蓝 `#1E3A5F` |
| Header 阴影 | 几乎不可见（弱化了） |
| 主标题 h1 | 字号 20px |

- [ ] **Step 4: Commit**

```bash
git add theme.css
git commit -m "style: theme § 1-2 base + header [fork-only]"
```

---

### Task 3: § 3 Top Storage Toolbar (inline 强覆盖)

**Files:**
- Modify: `theme.css` 第 3 段

- [ ] **Step 1: 写入 § 3 Top Storage Toolbar**

替换 `/* === 3. Top Storage Toolbar (override inline style) === */` 段：

```css
/* === 3. Top Storage Toolbar (override inline style) === */
body > div[style*="border-bottom:2px solid #edf2ff"] {
  background: #FFFFFF !important;
  border-bottom: 1px solid #E4E7EB !important;
  padding: 12px 24px !important;
  box-shadow: none !important;
}
body > div[style*="border-bottom:2px solid #edf2ff"] > span {
  font-size: 14px !important;
  color: #334155 !important;
}
body > div[style*="border-bottom:2px solid #edf2ff"] button {
  font-size: 14px !important;
  padding: 10px 18px !important;
  border-radius: 6px !important;
  transition: background 200ms ease !important;
}
body > div[style*="border-bottom:2px solid #edf2ff"] button[onclick*="saveData"] {
  background: #1E3A5F !important;
  color: #FFFFFF !important;
}
body > div[style*="border-bottom:2px solid #edf2ff"] button[onclick*="loadData"] {
  background: #FFFFFF !important;
  color: #334155 !important;
  border: 1.5px solid #CBD5E1 !important;
}
body > div[style*="border-bottom:2px solid #edf2ff"] button[onclick*="clearData"] {
  background: #FEF2F2 !important;
  color: #DC2626 !important;
  border: 1.5px solid #FECACA !important;
}
body > div[style*="border-bottom:2px solid #edf2ff"] #save-status {
  font-size: 12px !important;
  color: #059669 !important;
}
```

- [ ] **Step 2: 浏览器验证**

刷新页面，检查顶部"💾 数据存储"工具栏：

| 检查项 | 预期 |
|--------|------|
| 工具栏底边阴影 | 消失 / 改为 1px subtle border |
| "保存当前填写"按钮 | 颜色变深海军蓝 `#1E3A5F`（原本是更鲜亮的 `#1a56db`） |
| "加载上次填写"按钮 | 边框变 `#CBD5E1`，hover 时无紫色背景 |
| "清除数据"按钮 | 浅红底 `#FEF2F2` + 红字 `#DC2626` |
| 按钮高度 | 约 40px（DevTools Computed → height） |

- [ ] **Step 3: Commit**

```bash
git add theme.css
git commit -m "style: theme § 3 top toolbar inline override [fork-only]"
```

---

### Task 4: § 4 Progress Bar

**Files:**
- Modify: `theme.css` 第 4 段

- [ ] **Step 1: 写入 § 4 Progress Bar**

替换 `/* === 4. Progress Bar ================================= */` 段：

```css
/* === 4. Progress Bar ================================= */
.prog {
  border-radius: 8px;
  padding: 12px 16px;
  box-shadow: 0 1px 2px rgba(15,23,42,.05);
}
.dot {
  width: 36px;
  height: 36px;
  font-size: 13px;
  transition: background 200ms ease, color 200ms ease;
}
.dot.active { background: #1E3A5F; }
.dot.done   { background: #059669; }
.dot.idle   { background: #E4E7EB; color: #64748B; }
.ln.done    { background: #059669; }
.ln.idle    { background: #E4E7EB; }
.slabel {
  font-size: 12px;
  color: #64748B;
  margin-left: 8px;
}
```

- [ ] **Step 2: 浏览器验证**

刷新，看进度条区域：

| 检查项 | 预期 |
|--------|------|
| `.dot` 直径 | 36×36px（原 26×26） |
| 当前激活点 | 深海军蓝 `#1E3A5F`（原鲜蓝） |
| 已完成点 | 深绿 `#059669` |
| 待办点 | 灰 `#E4E7EB` |
| 整条进度条占用横向空间 | 比之前略宽，但 5 个点在 375px 屏幕上仍然不挤 |

- [ ] **Step 3: Commit**

```bash
git add theme.css
git commit -m "style: theme § 4 progress bar [fork-only]"
```

---

### Task 5: § 5 Cards & Sections + § 6 Form Fields

**Files:**
- Modify: `theme.css` 第 5、6 段

- [ ] **Step 1: 写入 § 5 Cards & Sections**

替换 `/* === 5. Cards & Sections ============================= */` 段：

```css
/* === 5. Cards & Sections ============================= */
.card {
  border-radius: 10px;
  box-shadow: 0 1px 2px rgba(15,23,42,.05);
  margin-bottom: 16px;
  padding: 20px;
}
.ctitle {
  font-size: 18px;
  color: #1E3A5F;
  border-bottom: 2px solid #EEF2FF;
  padding-bottom: 12px;
  margin-bottom: 16px;
}
.cbadge {
  font-size: 12px;
  background: #EEF2FF;
  color: #1E3A5F;
  padding: 2px 8px;
}
.divider {
  font-size: 14px;
  color: #475569;
  padding: 16px 0 12px;
  border-top: 1px dashed #E4E7EB;
}
.grid { gap: 12px; }
.fg { gap: 6px; }
```

- [ ] **Step 2: 写入 § 6 Form Fields**

替换 `/* === 6. Form Fields ================================== */` 段：

```css
/* === 6. Form Fields ================================== */
label {
  font-size: 14px;
  color: #334155;
}
.fn {
  font-size: 12px;
  background: #EEF2FF;
  color: #1E3A5F;
  padding: 1px 6px;
}
.hint {
  font-size: 12px;
  line-height: 1.45;
  color: #64748B;
}
.req { color: #DC2626; }
input[type=text],
input[type=date],
input[type=email],
input[type=tel],
select,
textarea {
  font-size: 16px;
  padding: 11px 12px;
  border: 1.5px solid #CBD5E1;
  border-radius: 6px;
  background: #FFFFFF;
  color: #0F172A;
  transition: border-color 200ms ease, box-shadow 200ms ease;
}
input:focus,
select:focus,
textarea:focus {
  outline: none;
  border-color: #1E3A5F;
  box-shadow: 0 0 0 3px rgba(30,58,95,.18);
  background: #FFFFFF;
}
textarea { min-height: 72px; }
```

- [ ] **Step 3: 浏览器验证（关键）**

刷新页面，进入"个人基本信息"卡片，检查：

| 检查项 | 预期 | DevTools 验证 |
|--------|------|---------------|
| input 字号 | 16px | 选中 input → Computed → font-size = 16px |
| input 高度 | ~44px | Computed → height ≈ 44 |
| input 边框 | `#CBD5E1`（浅灰，比原 `#e2e8f0` 略深） | Computed → border-color |
| input 聚焦环 | 深海军蓝 3px 环 + 边框变 `#1E3A5F` | 点击 input 测试 |
| label 字号 | 14px（原 12） | 直观可见变大 |
| `.fn` 编号角标 | 12px，深海军蓝配色 | |
| 卡片圆角 | 10px（原 12px，略减） | |
| 段标题（"👤 个人基本信息"） | 18px，深海军蓝 | |

iOS 自动缩放验证：DevTools 切换到 iPhone SE，点 input 不应触发 viewport 自动缩放（input ≥16px 时不会触发）。

- [ ] **Step 4: Commit**

```bash
git add theme.css
git commit -m "style: theme § 5-6 cards + form fields [fork-only]"
```

---

### Task 6: § 7 Radios + § 8 Buttons

**Files:**
- Modify: `theme.css` 第 7、8 段

- [ ] **Step 1: 写入 § 7 Radios / Checks**

替换 `/* === 7. Radios / Checks ============================== */` 段：

```css
/* === 7. Radios / Checks ============================== */
.ri, .ci {
  font-size: 14px;
  padding: 9px 14px;
  border-radius: 6px;
  border: 1.5px solid #CBD5E1;
  background: #FFFFFF;
  transition: border-color 200ms ease, background 200ms ease;
}
.ri:hover, .ci:hover {
  border-color: #1E3A5F;
  background: #EEF2FF;
}
.ri input, .ci input {
  accent-color: #1E3A5F;
  width: 14px;
  height: 14px;
}
```

- [ ] **Step 2: 写入 § 8 Buttons**

替换 `/* === 8. Buttons ====================================== */` 段：

```css
/* === 8. Buttons ====================================== */
.btn {
  font-size: 15px;
  padding: 11px 20px;
  border-radius: 6px;
  transition: background 200ms ease, border-color 200ms ease, color 200ms ease;
}
.btn-p {
  background: #1E3A5F;
  color: #FFFFFF;
}
.btn-p:hover { background: #15304D; }
.btn-s {
  background: #FFFFFF;
  color: #334155;
  border: 1.5px solid #CBD5E1;
}
.btn-s:hover {
  background: #EEF2FF;
  border-color: #1E3A5F;
  color: #1E3A5F;
}
.btn-dl {
  font-size: 16px;
  background: #059669;
  color: #FFFFFF;
  border-radius: 8px;
  padding: 12px 26px;
}
.btn-dl:hover { background: #047857; }
.btn-dl:disabled { opacity: .5; cursor: not-allowed; }
```

- [ ] **Step 3: 浏览器验证**

刷新页面，检查：

| 检查项 | 预期 |
|--------|------|
| 第 1 步性别单选项 `.ri` | 字号 14px，padding 充足，hover 时变浅紫底 + 深海军蓝边 |
| "下一步：旅行证件" 主按钮 | 深海军蓝 `#1E3A5F`，字号 15px，高度 ~44px |
| 主按钮 hover | 颜色更深 `#15304D` |
| （走到第 5 步后看）"导出 PDF" 按钮 `.btn-dl` | 深绿 `#059669`，字号 16px |

- [ ] **Step 4: Commit**

```bash
git add theme.css
git commit -m "style: theme § 7-8 radios + buttons [fork-only]"
```

---

### Task 7: § 9 Country Cards + § 10 Sumbox/Tip

**Files:**
- Modify: `theme.css` 第 9、10 段

- [ ] **Step 1: 写入 § 9 Country Cards**

替换 `/* === 9. Country Cards ================================ */` 段：

```css
/* === 9. Country Cards ================================ */
.country-card {
  padding: 14px 12px;
  border-radius: 8px;
  border: 2px solid #CBD5E1;
  transition: border-color 200ms ease, background 200ms ease, box-shadow 200ms ease;
}
.country-card:hover {
  border-color: #1E3A5F;
  background: #EEF2FF;
  transform: none;
  box-shadow: 0 1px 3px rgba(15,23,42,.08);
}
.country-card.selected {
  border-color: #1E3A5F;
  background: #EEF2FF;
  box-shadow: 0 0 0 3px rgba(30,58,95,.18);
}
.country-card.no-pdf:hover {
  border-color: #CBD5E1;
  background: #FFFFFF;
  transform: none;
  box-shadow: none;
}
.country-card.is-link {
  border-color: #F59E0B;
}
.country-card.is-link:hover {
  border-color: #D97706;
  background: #FFFBEB;
  transform: none;
  box-shadow: 0 1px 3px rgba(15,23,42,.08);
}
.country-name { font-size: 14px; }
.country-status { font-size: 12px; }
.status-ready { color: #059669; }
.status-pending { color: #F59E0B; }
.status-link { color: #D97706; }
```

- [ ] **Step 2: 写入 § 10 Sumbox / Tip**

替换 `/* === 10. Sumbox / Tip ================================ */` 段：

```css
/* === 10. Sumbox / Tip ================================ */
.sumbox {
  background: #F0FDF4;
  border: 1.5px solid #10B981;
  border-radius: 8px;
}
.sumbox h3 {
  font-size: 13px;
  color: #065F46;
}
.sumlbl { color: #065F46; }
.sumval { color: #047857; }
.sumrow { font-size: 12px; }
.tip {
  font-size: 12px;
  border-radius: 6px;
}
```

- [ ] **Step 3: 浏览器验证**

走到第 4 步（选择国家），检查：

| 检查项 | 预期 |
|--------|------|
| 国家卡 hover | **不再浮起**（`transform: none`），仅边框/背景变色 |
| 国家卡 selected | 深海军蓝边 + 浅紫底 + 3px ring |
| 国家卡 padding | 14×12（视觉略饱满） |

走到第 5 步（确认），检查：

| 检查项 | 预期 |
|--------|------|
| `.sumbox` 绿色摘要框 | 浅绿底 + 实色绿边（原本是浅绿点状边） |

- [ ] **Step 4: Commit**

```bash
git add theme.css
git commit -m "style: theme § 9-10 country cards + sumbox [fork-only]"
```

---

### Task 8: § 11 Responsive + § 12 Reduced Motion

**Files:**
- Modify: `theme.css` 第 11、12 段

- [ ] **Step 1: 写入 § 11 Responsive**

替换 `/* === 11. Responsive ================================== */` 段：

```css
/* === 11. Responsive ================================== */
@media (max-width: 580px) {
  .hdr { padding: 16px; }
  .container { padding: 16px 12px 64px; }
}
@media (min-width: 768px) {
  .container { max-width: 920px; }
  .hdr h1 { font-size: 22px; }
}
```

- [ ] **Step 2: 写入 § 12 Reduced Motion**

替换 `/* === 12. Reduced Motion ============================== */` 段：

```css
/* === 12. Reduced Motion ============================== */
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    animation-duration: .01ms !important;
    transition-duration: .01ms !important;
    scroll-behavior: auto !important;
  }
}
```

- [ ] **Step 3: 浏览器验证响应式**

刷新页面，DevTools 切换视口逐一验证：

| 视口 | 检查项 | 预期 |
|------|--------|------|
| iPhone SE (375px) | 容器单列、字号合适、不横向滚动 | ✓ |
| iPad (768px) | 容器居中、max-width = 920px、h1 = 22px | ✓ |
| Desktop (1280px) | 容器居中、max-width = 920px（不再加宽） | ✓ |

- [ ] **Step 4: 浏览器验证 reduced motion**

DevTools → ⋮ → More tools → Rendering → 找到 "Emulate CSS media feature prefers-reduced-motion" → 选 `reduce`。

| 检查项 | 预期 |
|--------|------|
| 国家卡 hover 过渡 | 几乎瞬时（无 200ms 平滑） |
| input focus ring 出现 | 几乎瞬时 |
| loading spinner（点导出 PDF 触发） | 几乎不旋转（`@keyframes spin` 的 `animation-duration` 被压成 .01ms） |

- [ ] **Step 5: Commit**

```bash
git add theme.css
git commit -m "style: theme § 11-12 responsive + reduced motion [fork-only]"
```

---

### Task 9: 综合验收（spec § 10）

**Files:**
- 无修改，仅验收

- [ ] **Step 1: 跑通完整 4 视口检查**

DevTools 逐一切换并完整跑一遍表单流程（5 步）：

| 视口 | 关键检查 |
|------|---------|
| iPhone SE 375×667 | 输入框、按钮、进度点不挤；input 聚焦不触发 viewport 缩放 |
| iPhone 14 390×844 | 同上 |
| iPad 768×1024 | 容器加宽到 920px；h1 22px |
| Desktop 1280×800 | 容器居中；视觉舒展 |

- [ ] **Step 2: 跑通 ui-ux-pro-max Pre-Delivery Checklist 抽查**

对照 § 1 (Accessibility) + § 6 (Typography) 抽查：

- [ ] body 字号 ≥16px ✓
- [ ] label 字号 ≥14px ✓
- [ ] 文本对比度 4.5:1（DevTools → Lighthouse → Accessibility）
- [ ] focus 可见
- [ ] reduced-motion 生效

- [ ] **Step 3: 验证 fork 同步契约**

```bash
git log --oneline a9d3cf6..HEAD
```

预期输出：约 8-9 个 commit，全部带 `[fork-only]` 标记，且 `index.html` 仅在 Task 1 那个 commit 改动 1 行。

```bash
git diff a9d3cf6..HEAD -- index.html
```

预期输出：只有 1 行 `+<link rel="stylesheet" href="theme.css">` 的新增。

- [ ] **Step 4: （可选）push 到 origin**

如本地验收满意，可选择推送：

```bash
git push origin main
```

如不 push，仅留本地。

- [ ] **Step 5: 写完成总结（无需新 commit）**

向用户报告：

- 改动行数：theme.css ~150 行新增、index.html 1 行新增
- commit 数量：约 9 个（全部 `[fork-only]`）
- 上游同步成本：每次 `merge upstream/main` 后只需检查 `<link>` 那行是否还在

---

## 自审记录

- ✅ 所有任务都给出确切代码片段
- ✅ 所有命令都给出预期输出
- ✅ 文件路径全为绝对路径或相对仓库根的明确路径
- ✅ 任务粒度合理：每个 task 5-15 分钟
- ✅ 每个 task 末尾都有 commit
- ✅ 验收方式可重复执行
- ✅ Spec § 1-11 全部覆盖：Task 2 (§1-2) / Task 3 (§3) / Task 4 (§4) / Task 5 (§5-6) / Task 6 (§7-8) / Task 7 (§9-10) / Task 8 (§11-12) / Task 9 (§ 10 验收)
- ✅ 类型一致性：颜色 hex、选择器名、属性选择器跨任务一致

