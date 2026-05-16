# 外交护照感装饰层实施计划

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** 在 [[2026-05-15-minimal-business-theme]] + [[2026-05-16-restrained-refinement]] 之上叠加外交护照感装饰层，落实 spec [`2026-05-16-diplomatic-passport-design.md`](../specs/2026-05-16-diplomatic-passport-design.md)。

**Architecture:** 在 `theme.css` 末尾追加新区段 `/* === 13. Diplomatic Passport Layer === */`，零影响 § 1–12 现有规则。5 个 commit 渐进追加：A Header → B Progress → C Counters → D Card → E Input/Stamps。

**Tech Stack:** 原生 CSS（伪元素、counter、box-shadow、内联 SVG data URL）。无 web font、无 JS、无 HTML 结构改动。

---

## File Structure

**唯一修改文件：** `theme.css`（仅末尾追加，不修改 § 1–12）

**新建文件：** 无

---

## Task 1: A · Header 双金线 + EU 圆章

**Files:**
- Modify: `theme.css` 末尾（在 `§ 12. Reduced Motion` 之后追加新区段 § 13）

- [ ] **Step 1: 在文件末尾追加 § 13 区段标头 + 13.A 规则**

old_string:
```css
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    animation-duration: .01ms !important;
    transition-duration: .01ms !important;
    scroll-behavior: auto !important;
  }
}
```

new_string:
```css
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    animation-duration: .01ms !important;
    transition-duration: .01ms !important;
    scroll-behavior: auto !important;
  }
}

/* === 13. Diplomatic Passport Layer ============ */

/* 13.A Header gold lines + EU emblem */
.hdr {
  position: relative;
  border-bottom: 1px solid #B8860B;
  padding-bottom: 28px;
}
.hdr::before {
  content: "";
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  height: 2px;
  background: linear-gradient(90deg, transparent 0%, #B8860B 20%, #C9A961 50%, #B8860B 80%, transparent 100%);
}
.hdr::after {
  content: "✦ EU · SCHENGEN VISA · APPLICATION ASSISTANT ✦";
  position: absolute;
  bottom: -8px;
  left: 50%;
  transform: translateX(-50%);
  background: #18181B;
  color: #C9A961;
  font-family: 'Songti SC', 'STSong', SimSun, Georgia, serif;
  font-size: 10px;
  letter-spacing: 0.2em;
  padding: 0 16px;
  white-space: nowrap;
}
.eu {
  background: #003399 !important;
  border-radius: 50% !important;
  width: 52px !important;
  height: 52px !important;
  padding: 0 !important;
  font-size: 0 !important;
  position: relative;
  box-shadow:
    inset 0 0 0 2px #FFD700,
    0 0 0 1px rgba(255,215,0,.3);
}
.eu::before {
  content: "EU";
  font-family: 'Songti SC', 'STSong', SimSun, Georgia, serif;
  font-size: 14px;
  font-weight: 600;
  letter-spacing: 0.08em;
  color: #FFD700;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}
```

- [ ] **Step 2: 验证**

Run:
```bash
grep -nE '13\. Diplomatic Passport Layer|13\.A Header gold' theme.css
```
Expected: 2 行命中

Run:
```bash
grep -nE '#B8860B|#C9A961|#003399|#FFD700' theme.css | wc -l
```
Expected: ≥ 8（13.A 段中暗金 4 处、暖金 2 处、欧盟蓝 1 处、金黄 3 处合计 ≥ 8）

Run:
```bash
grep -cE 'EU · SCHENGEN VISA · APPLICATION ASSISTANT' theme.css
```
Expected: 1

- [ ] **Step 3: Commit**

```bash
git add theme.css
git commit -m "style: passport A · header gold lines + EU emblem [fork-only]"
```

---

## Task 2: B · 步骤条衬线化 + 虚线连接

**Files:**
- Modify: `theme.css` 末尾（在 13.A 之后追加 13.B）

- [ ] **Step 1: 在 13.A 末尾追加 13.B 规则**

old_string（13.A 末尾段，作为 anchor）:
```css
.eu::before {
  content: "EU";
  font-family: 'Songti SC', 'STSong', SimSun, Georgia, serif;
  font-size: 14px;
  font-weight: 600;
  letter-spacing: 0.08em;
  color: #FFD700;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}
```

new_string:
```css
.eu::before {
  content: "EU";
  font-family: 'Songti SC', 'STSong', SimSun, Georgia, serif;
  font-size: 14px;
  font-weight: 600;
  letter-spacing: 0.08em;
  color: #FFD700;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}

/* 13.B Progress bar serif + dashed lines */
.dot {
  font-family: 'Songti SC', 'STSong', SimSun, Georgia, serif !important;
  font-weight: 500;
}
.dot.active {
  box-shadow:
    0 0 0 1px #B8860B,
    0 0 0 4px transparent,
    0 0 0 5px #C9A961;
}
.ln {
  background: transparent !important;
  position: relative;
  height: 2px;
}
.ln::before {
  content: "";
  position: absolute;
  top: 50%;
  left: 0;
  right: 0;
  border-top: 1px dashed;
  transform: translateY(-0.5px);
}
.ln.done::before { border-top-color: #16A34A; }
.ln.idle::before { border-top-color: #B8860B; opacity: 0.4; }
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

- [ ] **Step 2: 验证**

Run:
```bash
grep -nE '13\.B Progress bar' theme.css
```
Expected: 1 行命中

Run:
```bash
grep -cE 'border-top: 1px dashed' theme.css
```
Expected: 1（虚线连线）

Run:
```bash
grep -cE 'Songti SC' theme.css
```
Expected: ≥ 5（13.A: hdr::after / eu::before 2 处；13.B: dot / slabel 2 处；将累计）

- [ ] **Step 3: Commit**

```bash
git add theme.css
git commit -m "style: passport B · progress bar dashed + serif numbers [fork-only]"
```

---

## Task 3: C · 卡片标题自动编号

**Files:**
- Modify: `theme.css` 末尾（在 13.B 之后追加 13.C）

- [ ] **Step 1: 在 13.B 末尾追加 13.C 规则**

old_string:
```css
.slabel::before {
  content: "— ";
  color: #B8860B;
}
```

new_string:
```css
.slabel::before {
  content: "— ";
  color: #B8860B;
}

/* 13.C Section counters on .ctitle */
.container {
  counter-reset: section;
}
.ctitle {
  counter-increment: section;
  font-family: 'Songti SC', 'STSong', SimSun, '宋体', Georgia, 'PingFang SC', serif;
  position: relative;
  padding-right: 56px;
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

- [ ] **Step 2: 验证**

Run:
```bash
grep -nE '13\.C Section counters' theme.css
```
Expected: 1 行命中

Run:
```bash
grep -cE 'counter-reset: section|counter-increment: section|counter\(section, decimal-leading-zero\)' theme.css
```
Expected: 4（reset 1 + increment 1 + counter() 用 2 处）

- [ ] **Step 3: Commit**

```bash
git add theme.css
git commit -m "style: passport C · § section counters on titles [fork-only]"
```

---

## Task 4: D · 卡片纸感 + 顶部金线

**Files:**
- Modify: `theme.css` 末尾（在 13.C 之后追加 13.D）

- [ ] **Step 1: 在 13.C 末尾追加 13.D 规则**

⚠️ 注意：本 task 含一行较长的内联 SVG data URL，必须**完整一字不差**复制（包括 URL-encoded 字符 `%23` 和 `%25`）。

old_string:
```css
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

new_string:
```css
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

/* 13.D Card paper texture + gold border */
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

- [ ] **Step 2: 验证**

Run:
```bash
grep -nE '13\.D Card paper texture' theme.css
```
Expected: 1 行命中

Run:
```bash
grep -cE 'feTurbulence|fractalNoise' theme.css
```
Expected: 各 1（SVG noise 滤镜两个关键字）

Run:
```bash
grep -cE 'border-top: 1.5px solid #B8860B' theme.css
```
Expected: 1

- [ ] **Step 3: Commit**

```bash
git add theme.css
git commit -m "style: passport D · card paper texture + gold border [fork-only]"
```

---

## Task 5: E · Input 米色 + 状态印章

**Files:**
- Modify: `theme.css` 末尾（在 13.D 之后追加 13.E）

包含 spec § 3.E（Input + .fn）和 § 3.F（state stamps）两部分。

- [ ] **Step 1: 在 13.D 末尾追加 13.E 规则**

old_string:
```css
/* 13.D Card paper texture + gold border */
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

new_string:
```css
/* 13.D Card paper texture + gold border */
.card {
  background-color: #FFFFFF;
  background-image:
    linear-gradient(180deg, rgba(184,134,11,.04) 0%, transparent 80px),
    url("data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' width='80' height='80'><filter id='n'><feTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='1' stitchTiles='stitch'/><feColorMatrix values='0 0 0 0 0  0 0 0 0 0  0 0 0 0 0  0 0 0 .025 0'/></filter><rect width='100%25' height='100%25' filter='url(%23n)'/></svg>");
  background-repeat: no-repeat, repeat;
  background-size: 100% 80px, 80px 80px;
  border-top: 1.5px solid #B8860B;
}

/* 13.E Input parchment + .fn gold + state stamps */
input[type=text], input[type=date], input[type=email], input[type=tel],
select, textarea {
  background: #FFFDF7 !important;
}
input:focus, select:focus, textarea:focus {
  background: #FFFFFF !important;
}
.fn {
  font-family: 'Songti SC', 'STSong', SimSun, Georgia, serif !important;
  background: #FFF8E7 !important;
  color: #B8860B !important;
  border: 1px solid #C9A961 !important;
  letter-spacing: 0.06em !important;
}
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
.sumbox {
  position: relative;
  padding: 28px 16px 16px !important;
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

- [ ] **Step 2: 验证**

Run:
```bash
grep -nE '13\.E Input parchment' theme.css
```
Expected: 1 行命中

Run:
```bash
grep -cE '#FFFDF7|#FFF8E7' theme.css
```
Expected: ≥ 2（input 米色 1 处 + .fn 浅金底 1 处）

Run:
```bash
grep -cE 'EXAMINED' theme.css
```
Expected: 1

Run:
```bash
grep -cE '\.country-card\.selected::after' theme.css
```
Expected: 1

- [ ] **Step 3: Commit**

```bash
git add theme.css
git commit -m "style: passport E · input parchment + state stamps [fork-only]"
```

---

## Task 6: 浏览器目视回归（用户执行）

**用户操作。** 不在 implementer subagent 范围。

按 spec § 5 验证清单逐项目视：双金线、EU 圆章、宋体步骤数字、双圈金光、虚线连线、§01 P.01 自动编号、emoji 保留、卡片纸感、米色 input、金箔 .fn、selected ✓ 印章、EXAMINED 标签、移动端无回归、reduced-motion 仍生效。

如有问题定位到具体 Task 后修补再 commit。

---

## Self-Review

**1. Spec coverage:**

| Spec § | 覆盖 task |
|---|---|
| § 3.A Header 双金线 + EU 圆章 | Task 1 |
| § 3.B Progress Bar 衬线化 | Task 2 |
| § 3.C 卡片标题 § 序号 + 页码 | Task 3 |
| § 3.D 卡片本体纸感 | Task 4 |
| § 3.E 表单纸感 + .fn 金箔 | Task 5 |
| § 3.F 状态印章（country selected ✓ + sumbox EXAMINED） | Task 5 |
| § 5 验证清单 | Task 6（用户目视） |
| § 6 提交节奏（5 commit） | Task 1-5 各 1 commit |

无遗漏。`F` 的 2 个状态印章合并到 `Task 5 (E)` 是 spec § 6 钦定的（5 个 commit 而非 6 个）。

**2. Placeholder scan:** 每个 Edit 都给出完整 old_string / new_string。验证 grep 都给出确切命令与期望输出。无 TBD。SVG noise data URL 一字不差。

**3. Edit 顺序依赖:**
- Task 2/3/4/5 的 old_string 都用「上一个 task 末尾的几行」作为 anchor，所以累积修改不会 collision
- Task 5 的 old_string 是 13.D 整段（含完整 SVG），new_string 是 13.D 整段 + 13.E——SVG 字符串完全一致，即使复杂也不会出错
- Edit 工具按 string match 而非 line number，所以累积追加导致的行号位移不影响后续 task

**4. Type / value 一致性:**
- 字体 stack `'Songti SC', 'STSong', SimSun, Georgia, serif`（短）与 `'Songti SC', 'STSong', SimSun, '宋体', Georgia, 'PingFang SC', serif`（长，仅 .ctitle）—— spec § 2 钦定如此（.ctitle 中文为主，其他西文为主）
- 暗金 `#B8860B` 在 13.A/B/C/D/E 多处使用，全部一致
- 暖金 `#C9A961` 在 13.A（金线渐变）、13.B（dot.active 第三圈）、13.E（.fn 边）一致使用
- counter() 在 13.C 用了 2 次（::before 内 + ::after 内），都是 `counter(section, decimal-leading-zero)`，参数一致

**5. 跨规则覆盖正确性:**
- Task 5 的 input 背景规则用 `!important` 覆盖前层 § 6 的 `background: #FFFFFF`
- Task 5 的 .fn 规则用 `!important` 覆盖前层 § 6 的 `background: #F4F4F5; color: #18181B`
- focus 时回纯白也用 `!important` 覆盖 input:focus 既有 `background: #FFFFFF`（保险）
- .country-card.selected 加 `position: relative` 不冲突（前层未设 position）；spec [[2026-05-15-minimal-business-theme]] § 9 中 .country-card.selected 已有 box-shadow，不影响
- .sumbox 的 padding 用 `!important` 覆盖前层 § 10 的 padding（前层未显式设 padding，但 upstream `index.html` 内联可能有；保险起见用 !important）

无歧义。
