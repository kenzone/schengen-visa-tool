# 申根签证工具 · 政府文书数字化助手 主题设计

**日期：** 2026-05-15
**作用范围：** 视觉风格（颜色 / 字号 / 间距 / 触摸目标 / 阴影 / 圆角 / 响应式 / 减动效）
**不在范围：** 业务逻辑、HTML 结构、ARIA / a11y 修复、emoji→SVG 替换、构建工具引入

---

## 1. 背景与目标

当前 `index.html`（upstream `hazeljooooo/schengen-visa-tool` HEAD `a9d3cf6`）使用鲜亮蓝色 + 渐变 + 飘浮阴影的"产品页"风格，与"申根签证表单录入"的政府文书语境不匹配。

本设计把视觉语言从"产品页"切换到 **「政府文书数字化助手」** —— 沉稳、低饱和、弱光影、端庄圆角、可读性优先。

匹配 `ui-ux-pro-max` 的「Accessible & Ethical · 偏 Data-Dense」风格档位。

---

## 2. fork 维护契约（核心约束）

本仓库为 `hazeljooooo/schengen-visa-tool` 的 fork，需持续 `git merge upstream/main` 同步上游变更。本次改动遵循以下契约以将 merge 冲突压到最小：

- 主题改动以**独立文件 `theme.css`** 承载（fork 侧专属，upstream 永远不知道它存在）。
- `index.html` 内 `<style>` 块**一字不改**。
- `index.html` 唯一改动：`</head>` 前追加一行 `<link rel="stylesheet" href="theme.css">`。
- 该改动以独立 commit 提交，commit message 标记 `[fork-only]` 前缀，便于将来识别与 cherry-pick 取舍。
- 不引入 JS 改动、新依赖、构建工具、CSS 预处理器。

每次 `merge upstream/main` 后只需检查 `<link>` 那一行是否仍在原位即可。

---

## 3. 覆盖策略

`theme.css` 通过两种机制覆盖 upstream `<style>` 块：

1. **同选择器 + 后置定义**：theme.css 在 upstream `<style>` 之后加载，使用与原 CSS 完全相同的选择器名（`.hdr` / `.btn` / `.dot` 等）。CSS "后定义胜出"规则会自动覆盖。**不使用** `!important`。
2. **`!important` 强覆盖**：仅用于覆盖 `index.html` 行 100-106 顶部数据存储工具栏的 inline `style="..."`。该是 1 处而已。

不引入 CSS 变量重命名（原 CSS 没用变量，引变量等于额外抽象层，无收益）。

---

## 4. 设计 token

### 4.1 颜色

| 角色 | 当前值 | 新值 | 应用位置 |
|------|--------|------|---------|
| Primary | `#1a56db` | `#1E3A5F`（深海军蓝） | `.hdr` / `.btn-p` / focus ring / `.dot.active` / `.ctitle` / `.fn` / `.cbadge` |
| Primary hover | `#1541a0` | `#15304D` | `.btn-p:hover` |
| Header 背景 | `linear-gradient(135deg,#1a56db,#0e3a9e)` | `#1E3A5F` 纯色（去渐变） | `.hdr` |
| Accent（完成/导出） | `#10b981` | `#059669` | `.btn-dl` / `.dot.done` |
| Accent hover | `#059669` | `#047857` | `.btn-dl:hover` |
| Background | `#f0f4f8` | `#F8FAFC` | `body` |
| Foreground | `#1a202c` | `#0F172A` | 全局文本 |
| Border | `#e2e8f0` | `#CBD5E1` | input / 卡片 / 单选项 |
| Subtle border | — | `#E4E7EB` | 分割线 / 顶部工具栏底边 |
| Destructive | `#e53e3e` / `#c53030` | `#DC2626` | `.req` * / 错误 |
| Focus ring | `rgba(26,86,219,.1)` | `rgba(30,58,95,.18)` | `input:focus` |
| Sumbox 绿 | `#f0fdf4` / `#86efac` | `#F0FDF4` / `#10B981` | `.sumbox` |
| Tip 黄 | 保持 | 保持 | `.tip` |

### 4.2 字号阶梯

主体阶梯 **12 / 14 / 16 / 18 / 22**；过渡尺寸 13 仅用于 `.dot` 数字与 `.hdr p` 副标题。

| 元素 | 当前 | 新 |
|------|------|----|
| `body` 默认 | 未设 | **16px** / line-height 1.5 |
| `input` / `select` / `textarea` | 13px | **16px** （修复 iOS 自动缩放） |
| `label` | 12px | **14px** |
| `.hint` | 10px | **12px** |
| `.cbadge` | 10px | **12px** |
| `.fn` 字段编号 | 10px | **12px** |
| `.ctitle` 段标题 | 14px | **18px** |
| `.btn` | 13px | **15px** |
| `.btn-dl` | 14px | **16px** |
| 顶部工具栏 button | 12px | **14px** |
| `.dot` 数字 | 11px | **13px** |
| `.ri` / `.ci` | 12px | **14px** |
| `.country-name` | 12px | **14px** |
| `.country-status` | 10px | **12px** |
| `.hdr h1` | 18px | **20px**（mobile）/ **22px**（≥768px） |
| `.hdr p` | 12px | **13px** |

### 4.3 触摸目标（≥44px 高度）

| 元素 | 当前 padding | 新 padding | 高度 |
|------|------------|-----------|------|
| `input` / `select` | `7px 10px` (~32px) | `11px 12px` | ~44px |
| `textarea` min-height | 60px | **72px** | — |
| `.btn` | `8px 18px` (~30px) | `11px 20px` | ~44px |
| `.btn-dl` | `12px 26px` (~44px) | 保持 | ✓ |
| `.dot` 进度点 | 26×26 | **36×36** | ✓ |
| `.ri` / `.ci` | `5px 9px` (~26px) | `9px 14px` | ~38px（合理） |
| `.country-card` | `10px 8px` | `14px 12px` | — |
| 顶部工具栏 button | `7px 16px` (~30px) | `10px 18px` | ~40px |

### 4.4 间距

主体节奏 **4 / 8 / 12 / 16 / 24**。`.fg` gap 6px 与 `input/.btn` padding 11px 为达成"≥44px 触摸目标"做的微调，是节奏外的合理例外。

| 元素 | 当前 | 新 |
|------|------|----|
| `.container` padding | `18px 12px 60px` | `24px 16px 64px` |
| `.card` padding | 20px | 20px（保持） |
| `.card` margin-bottom | 12px | 16px |
| `.grid` gap | 11px | 12px |
| `.fg` gap (label↔input) | 4px | 6px |
| `.hdr` padding | `18px 28px` | `20px 24px` |
| `.prog` padding | `12px 18px` | `12px 16px` |
| `.nav` margin-top | 18px | 24px |
| `.divider` padding | `10px 0 6px` | `16px 0 12px` |
| 顶部工具栏 padding | `10px 28px` | `12px 24px` |

### 4.5 行高

```css
body                 { line-height: 1.5 }
h1, .ctitle          { line-height: 1.3 }
button               { line-height: 1 }
.hint                { line-height: 1.45 }
```

### 4.6 圆角

| 元素 | 当前 | 新 |
|------|------|----|
| `.card` | 12px | 10px |
| `.btn` | 8px | 6px |
| `.btn-dl` | 9px | 8px |
| `input` | 7px | 6px |
| `.ri` / `.ci` | 7px | 6px |
| `.country-card` | 9px | 8px |
| `.dot` | 50% | 保持 |

### 4.7 阴影

| 位置 | 当前 | 新 |
|------|------|----|
| `.hdr` | `0 4px 12px rgba(26,86,219,.3)` | `0 1px 3px rgba(15,23,42,.08)` |
| `.card` / `.prog` | `0 2px 6px rgba(0,0,0,.07)` | `0 1px 2px rgba(15,23,42,.05)` |
| `.country-card:hover` | `0 3px 8px rgba(26,86,219,.15)` + `translateY(-1px)` | `0 1px 3px rgba(15,23,42,.08)` （**取消 translateY**） |
| `input:focus` | `0 0 0 3px rgba(26,86,219,.1)` | `0 0 0 3px rgba(30,58,95,.18)` |

### 4.8 transition

当前 `0.15s / 0.18s / 0.2s` 混乱，统一为 **`200ms ease`**。

### 4.9 字体

不引入 Google Fonts。继续用系统字体栈：

```css
font-family: 'PingFang SC', 'Microsoft YaHei', system-ui, -apple-system, sans-serif;
```

---

## 5. inline style 强覆盖清单

仅 1 处需要 `!important`：

| 位置 | 内容 | 覆盖方式 |
|------|------|---------|
| `index.html` 行 100-106 | 顶部数据存储工具栏整段 inline `style="..."` + 内部 4 个 button | 属性选择器 `body > div[style*="border-bottom:2px solid #edf2ff"]` 配合内部 button 的属性选择器，`background` / `border` / `padding` / `box-shadow` 全用 `!important` 覆盖 |

其他 inline `style="display:none;..."`（性别/婚姻/证件类型 "其他" 输入框，行 168 / 182 / 197）是 JS 控制的功能性属性，**不动**。

---

## 6. 响应式

仅两个断点：

```css
@media (max-width: 580px) {
  /* 保留 upstream 单列规则；微调 .hdr / .container padding */
}
@media (min-width: 768px) {
  .container { max-width: 920px; }
  .hdr h1    { font-size: 22px; }
}
```

桌面端（>1024px）默认就用 920px 容器，不再加更宽断点。

---

## 7. 动效与 a11y

```css
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    animation-duration: .01ms !important;
    transition-duration: .01ms !important;
  }
}
```

`.country-card:hover` 的 `transform: translateY(-1px)` 一律取消（4.7 节已列入）。

本设计**不补全** a11y（label `for`、`<form>`、ARIA、错误反馈机制）—— 这些需要修改 HTML 结构，会与 fork 维护契约冲突，已在范围外。

---

## 8. theme.css 文件骨架

约 150 行，按 12 段组织：

```
/* 1. Base & Typography      */ body 字号/行高/背景
/* 2. Header                 */ .hdr 去渐变、配色、字号
/* 3. Top Storage Toolbar    */ 顶部工具栏 inline 覆盖（!important）
/* 4. Progress Bar           */ .dot 36×36、.ln 颜色
/* 5. Cards & Sections       */ .card 圆角/阴影、.ctitle 18px、.cbadge
/* 6. Form Fields            */ label / .fn / .hint / input 字号 16、padding、ring
/* 7. Radios / Checks        */ .ri / .ci 字号 / padding / 边框色
/* 8. Buttons                */ .btn / .btn-p / .btn-dl
/* 9. Country Cards          */ .country-card 取消 translateY
/* 10. Sumbox / Tip          */ 背景 / 边色
/* 11. Responsive            */ @media (min-width:768px) / (max-width:580px)
/* 12. Reduced Motion        */ @media (prefers-reduced-motion: reduce)
```

---

## 9. 文件清单

| 操作 | 文件 | 说明 |
|------|------|------|
| 新增 | `theme.css` | 主题文件，~150 行 |
| 新增 | `docs/superpowers/specs/2026-05-15-government-theme-design.md` | 本设计稿 |
| 修改 | `index.html` | 唯一改动：`</head>` 前加一行 `<link rel="stylesheet" href="theme.css">` |

提交策略：

- commit 1：本设计稿
- commit 2：`theme.css` + index.html 那一行 link，message 前缀 `[fork-only]`

---

## 10. 验收方式

无自动化测试，手动验收：

1. 本地直接 `open index.html`（项目无构建步骤）。
2. Chrome DevTools 切换至 **iPhone SE (375px)** / **iPhone 14 (390px)** / **iPad (768px)** / **桌面 1280px** 各跑一遍。
3. 关键检查：
   - input 聚焦时 iOS 不再触发自动缩放（验证 input `font-size: 16px` 生效）。
   - 进度点 36px 在 375px 屏幕上 5 个不挤、可点击。
   - 焦点环（`rgba(30,58,95,.18)`）可见、对比明显。
   - `.country-card:hover` 不再浮起。
   - 系统设置开"减少动效"后过渡几乎无延迟。
   - header 为纯深海军蓝，无渐变。
4. 对照 `ui-ux-pro-max` Pre-Delivery Checklist § 1 (Accessibility) + § 6 (Typography) 抽查通过。

---

## 11. 范围外（YAGNI）

明确不在本次改动内：

- HTML 结构调整（`<form>` 包裹、`<label for>` 关联、`<div>` 改 `<fieldset>` 等）
- ARIA / role / aria-live 错误广播
- emoji 图标替换为 SVG
- 错误反馈机制（inline error、submit 校验）
- 自动保存 / `beforeunload` 警告
- 暗色模式
- 拆分 PDF base64 数据出 index.html
- Google Fonts / 自定义 webfont
- 进度条加百分比 / 步骤名称
- 自动化测试 / E2E

如未来想做以上任意一项，需另起一份 spec、另立一份 fork 维护风险评估。
