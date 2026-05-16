# 精修克制风实施计划

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** 在 [[2026-05-15-minimal-business-theme]] 基础上叠加 5 组隐形精修，落实 spec [`2026-05-16-restrained-refinement-design.md`](../specs/2026-05-16-restrained-refinement-design.md)。

**Architecture:** 仅修改 `theme.css`，不动 upstream `index.html`、不引入 Web 字体、不引入新文件、不动色值。5 个 task = 5 个独立 commit，每个对应一个精修分组（A/B/C/D/E）。

**Tech Stack:** 原生 CSS，`Edit` 工具，浏览器目视回归。

---

## File Structure

**唯一修改文件：** `theme.css`

**新建文件：** 无

---

## Task 1: A · 字体渲染与数字精度

**Files:**
- Modify: `theme.css:9-15`（body 块），`theme.css:17-19`（§1 末尾）

- [ ] **Step 1: 在 body 块内追加 4 行字体渲染属性**

old_string:
```css
body {
  background: #FAFAFA;
  color: #09090B;
  font-size: 16px;
  line-height: 1.5;
  font-family: 'PingFang SC', 'Microsoft YaHei', system-ui, -apple-system, sans-serif;
}
```

new_string:
```css
body {
  background: #FAFAFA;
  color: #09090B;
  font-size: 16px;
  line-height: 1.5;
  font-family: 'PingFang SC', 'Microsoft YaHei', system-ui, -apple-system, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-rendering: optimizeLegibility;
  font-feature-settings: "kern" 1, "liga" 1;
}
```

- [ ] **Step 2: 在 §1 末尾新增 tabular-nums 规则（紧跟 `button { line-height: 1; }` 之后）**

old_string:
```css
button { line-height: 1; }

/* === 2. Header ======================================= */
```

new_string:
```css
button { line-height: 1; }
.dot, .sumval, .sumlbl, .sumrow, .fn {
  font-variant-numeric: tabular-nums;
}

/* === 2. Header ======================================= */
```

- [ ] **Step 3: 验证**

Run:
```bash
grep -nE 'font-smoothing|optimizeLegibility|font-feature-settings|font-variant-numeric' theme.css
```
Expected: 5 行命中（webkit-font-smoothing、moz-osx-font-smoothing、text-rendering、font-feature-settings、font-variant-numeric 各 1 行）

- [ ] **Step 4: Commit**

```bash
git add theme.css
git commit -m "style: refinement A · font rendering + tabular-nums [fork-only]"
```

---

## Task 2: B · 空间节奏 +4px

**Files:**
- Modify: `theme.css:94-106`（§5 .card 与 .ctitle），`theme.css:120`（§5 .grid）

- [ ] **Step 1: 调整 .card 的 padding 与 margin-bottom**

old_string:
```css
.card {
  border-radius: 10px;
  box-shadow: 0 1px 2px rgba(15,23,42,.05);
  margin-bottom: 16px;
  padding: 20px;
}
```

new_string:
```css
.card {
  border-radius: 10px;
  box-shadow: 0 1px 2px rgba(15,23,42,.05);
  margin-bottom: 20px;
  padding: 24px;
}
```

- [ ] **Step 2: 调整 .ctitle 的 padding-bottom 与 margin-bottom**

old_string:
```css
.ctitle {
  font-size: 18px;
  color: #18181B;
  border-bottom: 2px solid #F4F4F5;
  padding-bottom: 12px;
  margin-bottom: 16px;
}
```

new_string:
```css
.ctitle {
  font-size: 18px;
  color: #18181B;
  border-bottom: 2px solid #F4F4F5;
  padding-bottom: 16px;
  margin-bottom: 20px;
}
```

注：`.ctitle` 的 `font-size: 18px` 暂不动，留到 Task 3 处理。

- [ ] **Step 3: 调整 .grid gap**

old_string: `.grid { gap: 12px; }`
new_string: `.grid { gap: 14px; }`

- [ ] **Step 4: 验证**

Run:
```bash
grep -nE 'padding: 24px|margin-bottom: 20px|padding-bottom: 16px|gap: 14px' theme.css
```
Expected: 至少 4 行命中（.card padding、.card margin、.ctitle padding-bottom、.grid gap；.ctitle margin-bottom 也是 20px 但与 .card margin 行重）

Run:
```bash
grep -nE 'padding: 20px$|margin-bottom: 16px|padding-bottom: 12px;|gap: 12px' theme.css
```
Expected: 无输出（旧值已消失）

- [ ] **Step 5: Commit**

```bash
git add theme.css
git commit -m "style: refinement B · spacing rhythm +4px [fork-only]"
```

---

## Task 3: C · 字号与字距精修

**Files:**
- Modify: `theme.css:25-28`（§2 .hdr h1），`theme.css:87-91`（§4 .slabel），`theme.css:100-106`（§5 .ctitle），`theme.css:124-127`（§6 label），`theme.css:128-133`（§6 .fn），`theme.css:277-280`（§11 桌面 .hdr h1）

5 处独立 Edit。

- [ ] **Step 1: .hdr h1 字号 + 字距**

old_string:
```css
.hdr h1 {
  font-size: 20px;
  line-height: 1.3;
}
```

new_string:
```css
.hdr h1 {
  font-size: 22px;
  line-height: 1.3;
  letter-spacing: -0.01em;
}
```

- [ ] **Step 2: .slabel 字距**

old_string:
```css
.slabel {
  font-size: 12px;
  color: #71717A;
  margin-left: 8px;
}
```

new_string:
```css
.slabel {
  font-size: 12px;
  color: #71717A;
  margin-left: 8px;
  letter-spacing: 0.02em;
}
```

- [ ] **Step 3: .ctitle 字号 + 字距**

old_string:
```css
.ctitle {
  font-size: 18px;
  color: #18181B;
  border-bottom: 2px solid #F4F4F5;
  padding-bottom: 16px;
  margin-bottom: 20px;
}
```

注意：上方 `padding-bottom` 与 `margin-bottom` 是 Task 2 已经改过的新值，所以 old_string 里写新值。

new_string:
```css
.ctitle {
  font-size: 19px;
  color: #18181B;
  border-bottom: 2px solid #F4F4F5;
  padding-bottom: 16px;
  margin-bottom: 20px;
  letter-spacing: -0.005em;
}
```

- [ ] **Step 4: label 字距**

old_string:
```css
label {
  font-size: 14px;
  color: #3F3F46;
}
```

new_string:
```css
label {
  font-size: 14px;
  color: #3F3F46;
  letter-spacing: 0.01em;
}
```

- [ ] **Step 5: .fn 字距 + uppercase**

old_string:
```css
.fn {
  font-size: 12px;
  background: #F4F4F5;
  color: #18181B;
  padding: 1px 6px;
}
```

new_string:
```css
.fn {
  font-size: 12px;
  background: #F4F4F5;
  color: #18181B;
  padding: 1px 6px;
  letter-spacing: 0.04em;
  text-transform: uppercase;
}
```

- [ ] **Step 6: §11 桌面 .hdr h1 字号 22→24**

old_string:
```css
@media (min-width: 768px) {
  .container { max-width: 920px; }
  .hdr h1 { font-size: 22px; }
}
```

new_string:
```css
@media (min-width: 768px) {
  .container { max-width: 920px; }
  .hdr h1 { font-size: 24px; }
}
```

- [ ] **Step 7: 验证**

Run:
```bash
grep -nE 'letter-spacing: -0.01em|letter-spacing: -0.005em|letter-spacing: 0.01em|letter-spacing: 0.02em|letter-spacing: 0.04em|text-transform: uppercase' theme.css
```
Expected: 6 行命中

Run:
```bash
grep -nE 'font-size: 22px|font-size: 19px|font-size: 24px' theme.css
```
Expected: 至少 3 行命中（h1 22px、ctitle 19px、桌面 h1 24px；其他 22px/19px/24px 出现也算）

Run:
```bash
grep -nE 'font-size: 18px;' theme.css
```
Expected: 无输出（.ctitle 旧字号已消失）

- [ ] **Step 8: Commit**

```bash
git add theme.css
git commit -m "style: refinement C · type sizing + letter-spacing [fork-only]"
```

---

## Task 4: D · 过渡曲线统一替换

**Files:**
- Modify: `theme.css` 6 处 transition 声明

替换所有 `200ms ease` 为 `200ms cubic-bezier(0.16, 1, 0.3, 1)`。

- [ ] **Step 1: §3 toolbar button transition**

old_string:
```css
  transition: background 200ms ease !important;
```

new_string:
```css
  transition: background 200ms cubic-bezier(0.16, 1, 0.3, 1) !important;
```

- [ ] **Step 2: §4 .dot transition**

old_string:
```css
  transition: background 200ms ease, color 200ms ease;
```

new_string:
```css
  transition: background 200ms cubic-bezier(0.16, 1, 0.3, 1), color 200ms cubic-bezier(0.16, 1, 0.3, 1);
```

- [ ] **Step 3: §6 input/select/textarea transition**

old_string:
```css
  transition: border-color 200ms ease, box-shadow 200ms ease;
```

new_string:
```css
  transition: border-color 200ms cubic-bezier(0.16, 1, 0.3, 1), box-shadow 200ms cubic-bezier(0.16, 1, 0.3, 1);
```

- [ ] **Step 4: §7 .ri/.ci transition**

old_string:
```css
  transition: border-color 200ms ease, background 200ms ease;
```

new_string:
```css
  transition: border-color 200ms cubic-bezier(0.16, 1, 0.3, 1), background 200ms cubic-bezier(0.16, 1, 0.3, 1);
```

- [ ] **Step 5: §8 .btn transition**

old_string:
```css
  transition: background 200ms ease, border-color 200ms ease, color 200ms ease;
```

new_string:
```css
  transition: background 200ms cubic-bezier(0.16, 1, 0.3, 1), border-color 200ms cubic-bezier(0.16, 1, 0.3, 1), color 200ms cubic-bezier(0.16, 1, 0.3, 1);
```

- [ ] **Step 6: §9 .country-card transition**

old_string:
```css
  transition: border-color 200ms ease, background 200ms ease, box-shadow 200ms ease;
```

new_string:
```css
  transition: border-color 200ms cubic-bezier(0.16, 1, 0.3, 1), background 200ms cubic-bezier(0.16, 1, 0.3, 1), box-shadow 200ms cubic-bezier(0.16, 1, 0.3, 1);
```

- [ ] **Step 7: 验证**

Run:
```bash
grep -nE '200ms ease' theme.css
```
Expected: 无输出（所有 ease 已替换）

Run:
```bash
grep -cE 'cubic-bezier\(0\.16, 1, 0\.3, 1\)' theme.css
```
Expected: ≥ 13（6 处 transition × 各自包含的属性数：1+2+2+2+3+3=13；命中 13 次）

- [ ] **Step 8: Commit**

```bash
git add theme.css
git commit -m "style: refinement D · transition curves → cubic-bezier [fork-only]"
```

---

## Task 5: E · 按下反馈与卡片 lift

**Files:**
- Modify: `theme.css:213-215`（§8 末尾追加 :active 规则），`theme.css:222-227`（§9 .country-card:hover）

- [ ] **Step 1: §8 末尾追加 .btn / .btn-dl 的 :active 规则**

old_string:
```css
.btn-dl:disabled { opacity: .5; cursor: not-allowed; }

/* === 9. Country Cards ================================ */
```

new_string:
```css
.btn-dl:disabled { opacity: .5; cursor: not-allowed; }
.btn:active, .btn-dl:active {
  transform: scale(0.98);
  transition: transform 120ms cubic-bezier(0.16, 1, 0.3, 1);
}

/* === 9. Country Cards ================================ */
```

- [ ] **Step 2: §9 .country-card:hover 改 transform 与 box-shadow**

old_string:
```css
.country-card:hover {
  border-color: #18181B;
  background: #FAFAFA;
  transform: none;
  box-shadow: 0 1px 3px rgba(15,23,42,.08);
}
```

new_string:
```css
.country-card:hover {
  border-color: #18181B;
  background: #FAFAFA;
  transform: translateY(-1px);
  box-shadow: 0 2px 6px rgba(15,23,42,.06);
}
```

注：`.country-card.no-pdf:hover` 与 `.country-card.is-link:hover` 仍保留 `transform: none`，按 spec § 1.E 要求差异化。

- [ ] **Step 3: 验证**

Run:
```bash
grep -nE '\.btn:active|transform: scale\(0\.98\)|transform: translateY\(-1px\)' theme.css
```
Expected: 至少 3 行命中

Run:
```bash
grep -nE 'transform: none' theme.css
```
Expected: 2 行命中（.country-card.no-pdf:hover 和 .country-card.is-link:hover——保留差异）

Run:
```bash
grep -nE 'box-shadow: 0 2px 6px rgba\(15,23,42,\.06\)' theme.css
```
Expected: 1 行命中（.country-card:hover 新阴影）

- [ ] **Step 4: Commit**

```bash
git add theme.css
git commit -m "style: refinement E · button press + card lift [fork-only]"
```

---

## Task 6: 浏览器目视回归（用户执行）

**用户操作。** 不在 implementer subagent 范围。

按 spec § 3 验证清单逐项目视：mac 抗锯齿、tabular-nums、节奏放松、Header 字号抬高、`.fn` 大写、过渡曲线、按钮按下、国家卡上浮、is-link 卡不上浮、reduced-motion 退化、移动端无回归。

如有问题，定位到具体 Task 后修补再 commit。

---

## Self-Review

**1. Spec coverage:**

| Spec § | 覆盖 task |
|---|---|
| §1.A 字体渲染与数字 | Task 1（2 处 Edit） |
| §1.B 空间节奏 | Task 2（3 处 Edit） |
| §1.C 字号与字距 | Task 3（6 处 Edit） |
| §1.D 过渡曲线 | Task 4（6 处 Edit） |
| §1.E 按下与卡片 lift | Task 5（2 处 Edit） |
| §3 验证清单 | Task 6（用户目视） |
| §4 提交节奏（5 commit） | Task 1-5 各 1 commit |

无遗漏。

**2. Placeholder scan:** 每个 Edit 都给出完整的 old_string / new_string，验证 grep 都给出确切命令与期望输出。无 TBD。

**3. Type / value 一致性:**
- `cubic-bezier(0.16, 1, 0.3, 1)` 在 Task 4 出现 ≥ 13 次 + Task 5 :active 规则 1 次，参数完全一致
- Task 2 改了 `.ctitle padding-bottom 16px / margin-bottom 20px`；Task 3 Step 3 的 old_string 也是新值 16px / 20px——一致，不会冲突
- Task 5 Step 2 替换的 `transform: none` 仅替换 `.country-card:hover` 一处；`.no-pdf:hover` 和 `.is-link:hover` 的 `transform: none` 保留——验证 grep 期望 2 行命中正是这两处
- Task 4 Step 1 toolbar 选择器内有 `!important`，new_string 保留 `!important`——一致

**4. Edit 顺序依赖:**
- Task 2 Step 2 改 .ctitle 的 padding-bottom 和 margin-bottom，Task 3 Step 3 再改 .ctitle 的 font-size 和 letter-spacing——old_string 互相不冲突（Task 3 用的是 Task 2 之后的状态）
- Task 1 Step 2 在 §1 末尾插入新规则，不影响后续 task 的行号定位（Edit 工具按字符串而非行号匹配）

无问题。
