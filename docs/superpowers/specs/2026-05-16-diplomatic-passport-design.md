# 外交护照感（Diplomatic Passport）装饰层设计稿

**日期：** 2026-05-16
**作用对象：** `theme.css`（fork-only 主题层）
**前置：**
- [[2026-05-15-minimal-business-theme-design]]（色彩底层）
- [[2026-05-16-restrained-refinement-design]]（精修层）

## 0. 背景与目标

前两层把骨架（极简黑）和细节（抗锯齿、tabular-nums、cubic-bezier）都做完。本稿在不动骨架的前提下，叠加一层「外交护照」装饰——把这个工具从「填表助手」视觉升级为「准官方文件填写流程」。

**记忆点：** 让用户在点击「下一步」时有一种「翻护照内页」的感觉。

**核心审美参照：**
- 申根签证本身（蓝护照 + 暗金箔字）
- 欧盟旗与外交印章
- 民国/外交公文（中文宋体的庄重感）

**叠加而非替换：** 现有 `§ 1–12` 全部保留不动；本稿在 `theme.css` 末尾追加新区段 `§ 13. Diplomatic Passport Layer`。

**用户决策（已拍板）：**
1. 不引入 web font，使用系统衬线 stack
2. emoji 保留作为「章节图标」参与构图
3. input 背景改米色（护照纸感核心承载）

## 1. 新增装饰色（4 个 token）

| Token | 值 | 用途 |
|---|---|---|
| 暗金 | `#B8860B` | 细线、§ 序号、页码、印章字、.fn 边/字 |
| 暖金 | `#C9A961` | 高光（金线中段渐变、selected 角标补色） |
| 欧盟蓝 | `#003399` | 仅 `.eu` 圆形徽章底 |
| 护照米 | `#FFFDF7` | input 平时背景、.card 顶端微染色 |

**纪律：** 这 4 个 token 是装饰色，**不参与品牌主色系统**。主色仍然是 `#18181B`，焦点蓝仍然是 `#2563EB`。

## 2. 字体栈（不引入 web font）

所有装饰处统一使用同一 stack（硬编码不引入 CSS 变量，沿用 spec [[2026-05-15-minimal-business-theme-design]] § 3.1 纪律）：

```css
font-family: 'Songti SC', 'STSong', SimSun, '宋体', Georgia, 'PingFang SC', serif;
```

| 系统 | 中文渲染 | 西文渲染 |
|---|---|---|
| macOS / iOS | Songti SC（思源宋体） | Songti SC 内置西文衬线 |
| Windows | SimSun / 宋体 | Georgia |
| Linux | serif fallback | Georgia |

应用点：`.hdr h1` / `.hdr p` / `.ctitle` / `.dot` / `.cbadge` / `.fn` / `.slabel` / `.sumbox h3` / 所有伪元素装饰文字。

非装饰区（label、hint、input 内文字、按钮文字）继续 PingFang 黑体。

## 3. 六组装饰

### A. Header 双线 + EU 徽章

#### A.1 顶部金色细线（::before）
```css
.hdr {
  position: relative;
  border-bottom: 1px solid #B8860B;
  padding-bottom: 28px;  /* 给底部装饰小字让位 */
}
.hdr::before {
  content: "";
  position: absolute;
  top: 0; left: 0; right: 0;
  height: 2px;
  background: linear-gradient(90deg,
    transparent 0%, #B8860B 20%, #C9A961 50%, #B8860B 80%, transparent 100%);
}
```
**视觉：** 顶部一条 2px 金线，从透明渐入 → 暗金 → 暖金高光 → 暗金 → 透明渐出，模拟"压金箔"工艺。

#### A.2 底部装饰小字（::after）
```css
.hdr::after {
  content: "✦ EU · SCHENGEN VISA · APPLICATION ASSISTANT ✦";
  position: absolute;
  bottom: -8px;        /* 浮于 border-bottom 上方 */
  left: 50%;
  transform: translateX(-50%);
  background: #18181B; /* 与 Header 同色，遮挡 border */
  color: #C9A961;
  font-family: 'Songti SC', 'STSong', SimSun, Georgia, serif;
  font-size: 10px;
  letter-spacing: 0.2em;
  padding: 0 16px;
  white-space: nowrap;
}
```
**视觉：** Header 底缘金色细线被一段"✦ EU · SCHENGEN VISA · APPLICATION ASSISTANT ✦"小字打断居中——像护照封面的国徽下方铭文。

#### A.3 `.eu ★★` 改造为欧盟徽章
upstream HTML：`<div class="eu">★★</div>`，upstream CSS 已设 background: #003399、border-radius: 5px、color: #FFD700。

改造（保留语义但视觉升级）：
```css
.eu {
  background: #003399 !important;
  border-radius: 50% !important;       /* 圆形 */
  width: 52px !important;
  height: 52px !important;
  padding: 0 !important;
  font-size: 0 !important;             /* 隐藏 ★★ 文字 */
  position: relative;
  box-shadow:
    inset 0 0 0 2px #FFD700,           /* 内圈金边 */
    0 0 0 1px rgba(255,215,0,.3);     /* 外圈微金光 */
}
.eu::before {
  content: "EU";
  font-family: 'Songti SC', 'STSong', SimSun, Georgia, serif;
  font-size: 14px;
  font-weight: 600;
  letter-spacing: 0.08em;
  color: #FFD700;
  position: absolute;
  top: 50%; left: 50%;
  transform: translate(-50%, -50%);
}
```
**已知妥协：** 不做完整 12 星圆环（12 个 box-shadow 点过于复杂、且小尺寸下星形细节看不清）。改为「EU」金字徽章——视觉密度更高，与小尺寸相容。

### B. Progress Bar 衬线化

```css
/* 步骤数字字体 */
.dot {
  font-family: 'Songti SC', 'STSong', SimSun, Georgia, serif !important;
  font-weight: 500;
}

/* active 双圈金光 */
.dot.active {
  box-shadow:
    0 0 0 1px #B8860B,
    0 0 0 4px transparent,
    0 0 0 5px #C9A961;
}

/* 连线虚线化 */
.ln {
  background: transparent !important;  /* 取消原实线背景 */
  position: relative;
  height: 2px;
}
.ln::before {
  content: "";
  position: absolute;
  top: 50%; left: 0; right: 0;
  border-top: 1px dashed;
  transform: translateY(-0.5px);
}
.ln.done::before { border-top-color: #16A34A; }
.ln.idle::before { border-top-color: #B8860B; opacity: 0.4; }

/* 步骤标签衬线斜体 + 破折号 */
.slabel {
  font-family: 'Songti SC', 'STSong', SimSun, Georgia, serif;
  font-style: italic;
  letter-spacing: 0.04em;
}
.slabel::before {
  content: "— ";
  color: #B8860B;
}
```
**视觉：** 步骤数字"1 2 3 4 5"变宋体；active 步骤外圈有 1px 暗金 + 1px 透明 + 1px 暖金的"双圈"印章感；步骤间连线变虚线（已完成绿，未达暗金低透）；步骤名前面加"— "破折号 + 衬线斜体。

### C. 卡片标题 § 序号 + 页码

upstream HTML 形如：`<div class="ctitle">👤 个人基本信息 <span class="cbadge">第 1–12 项</span></div>`。upstream CSS 已设 `display: flex; align-items: center; gap: 7px`。

策略：CSS counter 自动从 01 编号到 05；序号 ::before 作为 flex item，页码 ::after 用绝对定位避免挤压 cbadge。

```css
/* container 是步骤容器，5 张卡都是 .container 直接子孙 */
.container {
  counter-reset: section;
}

.ctitle {
  counter-increment: section;
  font-family: 'Songti SC', 'STSong', SimSun, '宋体', Georgia, 'PingFang SC', serif;
  position: relative;
  padding-right: 56px;  /* 给页码 ::after 让位 */
}

.ctitle::before {
  content: "§ " counter(section, decimal-leading-zero) " ─ ";
  color: #B8860B;
  font-weight: 500;
  letter-spacing: 0.05em;
  font-style: normal;
}

.ctitle::after {
  content: "P." counter(section, decimal-leading-zero);
  position: absolute;
  right: 0;
  top: 50%;
  transform: translateY(-50%);
  color: #B8860B;
  font-style: italic;
  font-size: 13px;
  letter-spacing: 0.05em;
}
```

**最终视觉：** 「§ 01 ─ 👤 个人基本信息 第 1–12 项                 P.01」（cbadge 在中间，页码贴最右）。

### D. 卡片本体纸感

```css
.card {
  background-color: #FFFFFF;
  background-image:
    linear-gradient(180deg, rgba(184,134,11,.04) 0%, transparent 80px),
    url("data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' width='80' height='80'><filter id='n'><feTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='1' stitchTiles='stitch'/><feColorMatrix values='0 0 0 0 0  0 0 0 0 0  0 0 0 0 0  0 0 0 .025 0'/></filter><rect width='100%25' height='100%25' filter='url(%23n)'/></svg>");
  background-repeat: no-repeat, repeat;
  background-size: 100% 80px, 80px 80px;
  border-top: 1.5px solid #B8860B;
}
```

**视觉：**
1. 顶部 80px 高的 4% 暗金渐变 → 营造「页眉」暖光
2. 整张卡片叠加 2.5% opacity 的 SVG noise → 纸纤维感（mac/iOS 高 DPI 屏可见，Win 低 DPI 屏几乎不可见，零功能影响）
3. 卡片顶部 1.5px 暗金细线 → 与下方 zinc-200 边框组成「双层护照内页」线

### E. 表单纸感 + 字段编号金箔

```css
/* input 米色护照纸感 */
input[type=text], input[type=date], input[type=email], input[type=tel],
select, textarea {
  background: #FFFDF7 !important;
}
/* focus 时回纯白突出"正在填" */
input:focus, select:focus, textarea:focus {
  background: #FFFFFF !important;
}

/* .fn 字段编号金箔感 */
.fn {
  font-family: 'Songti SC', 'STSong', SimSun, Georgia, serif !important;
  background: #FFF8E7 !important;          /* 浅金底 */
  color: #B8860B !important;
  border: 1px solid #C9A961 !important;
  letter-spacing: 0.06em !important;
  /* 注意 .fn 已经在精修层 spec 里加了 letter-spacing: 0.04em + uppercase；此处 letter-spacing 提到 0.06em，覆盖 */
}
```

**视觉：** input 平时是米色护照纸，focus 时变纯白（暗示"我在填"）；字段编号 `PD-1` `FN-2` 像盖在表上的金箔印章。

### F. 状态印章

```css
/* selected 国家卡：右上 ✓ 金印 */
.country-card.selected {
  position: relative;
}
.country-card.selected::after {
  content: "✓";
  position: absolute;
  top: 4px;
  right: 8px;
  font-family: 'Songti SC', 'STSong', SimSun, Georgia, serif;
  font-size: 18px;
  font-weight: 700;
  color: #B8860B;
  pointer-events: none;
}

/* sumbox 右上：— EXAMINED · */
.sumbox {
  position: relative;
  padding: 28px 16px 16px !important;  /* 顶部加 padding 给 ::before 让位 */
}
.sumbox::before {
  content: "— EXAMINED ·";
  position: absolute;
  top: 10px;
  right: 14px;
  font-family: 'Songti SC', 'STSong', SimSun, Georgia, serif;
  font-size: 10px;
  letter-spacing: 0.22em;
  color: #B8860B;
  font-weight: 500;
}
```

**视觉：** 选中的国家卡右上角出现 ✓ 金印；汇总框右上角出现「— EXAMINED ·」金色小字（像签证官审完盖的章）。

## 4. 实施约束

1. **追加而非修改：** 所有新规则放在 `theme.css` 末尾新区段 `/* === 13. Diplomatic Passport Layer ============ */`，不改动 § 1–12 任何现有规则（除了 `.eu`、`.dot`、`.ln`、`.slabel`、`.fn`、`.card`、`input`、`.sumbox`、`.country-card.selected` 等需要 `!important` 覆盖时的特殊处理——但写法依然是新的覆盖规则，不在原区段内修改）。

2. **CSS counter 重置在 `.container`：** index.html 中 5 张步骤卡片是 `.container > .card`，但 `.ctitle` 在 `.card` 内。在 `.container` 上 `counter-reset: section` + 在 `.ctitle` 上 `counter-increment: section`，保证 §01 → §05 正确编号。

3. **`prefers-reduced-motion` 已自动覆盖：** § 12 块对 `*` 强制 `transition-duration: .01ms`，本稿没有动效，不需额外适配。

4. **不动 web font：** 用户已确认走系统字体栈。fallback 链确保 Linux/Win 也能"接近"看到衬线。

5. **emoji 保留：** `.ctitle` 内 emoji 是文字节点不可剥离，保留作为"章节图标"，与 §01 衬线序号、cbadge、页码组成「§01 ─ 👤 个人基本信息 第 1–12 项 P.01」格式。

6. **Header `.eu` 改造：** 不做完整 12 星圆环，简化为 EU 金字圆章。妥协理由：12 个 box-shadow 点视觉密度过低、且 52px 容器内星形细节模糊。

## 5. 验证清单

- [ ] mac 浏览器中 Header 顶部有金色细线（左右渐入渐出）
- [ ] Header 底部有 `✦ EU · SCHENGEN VISA · APPLICATION ASSISTANT ✦` 金色小字（居中浮于 border 上）
- [ ] Header 左侧 `<div class="eu">` 显示为蓝色圆形 + EU 金字
- [ ] 步骤数字 1–5 显示为衬线宋体（mac 上明显）
- [ ] 当前步骤外圈有金色双圈
- [ ] 步骤之间连线为虚线（已完成绿，未达暗金低透）
- [ ] 步骤标签前有"— " 破折号 + italic 宋体
- [ ] 每个 .ctitle 前面有「§ 01 ─ 」 暗金衬线序号（自动 01 → 05）
- [ ] 每个 .ctitle 末尾右侧有「P.01」 金色 italic 页码
- [ ] .ctitle 中间的 emoji（👤🛂💼🗓️✅）仍然显示
- [ ] 卡片底色有极弱纸感纹理（mac 高 DPI 可见）
- [ ] 卡片顶部有暗金细线
- [ ] 卡片顶部 80px 有微暖光渐变
- [ ] input 平时背景米色，焦点时变纯白
- [ ] .fn 字段编号显示为浅金底 + 暗金边 + 暗金衬线字
- [ ] 国家卡 selected 时右上角有 `✓` 金色印章
- [ ] Sumbox 右上角有「— EXAMINED ·」金色小字
- [ ] 移动端（≤580px）布局无回归
- [ ] DevTools `prefers-reduced-motion: reduce` 仍生效

## 6. 提交节奏

5 个 commit：

```
style: passport A · header gold lines + EU emblem [fork-only]
style: passport B · progress bar dashed + serif numbers [fork-only]
style: passport C · § section counters on titles [fork-only]
style: passport D · card paper texture + gold border [fork-only]
style: passport E · input parchment + state stamps [fork-only]
```
