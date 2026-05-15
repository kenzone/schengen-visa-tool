# 极简商务主题实施计划

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** 把 `theme.css` 从「政府文书风（深海军蓝 #1E3A5F + 强调绿 #059669）」迁移到「极简商务风（曜黑 #18181B + 蓝焦点 #2563EB + 状态色保留）」，落实 spec [`2026-05-15-minimal-business-theme-design.md`](../specs/2026-05-15-minimal-business-theme-design.md)。

**Architecture:** 纯 CSS 色值替换。仅动 `theme.css` 一个文件，不动 upstream `index.html`。按 spec 的 §1–§12 编号分多次小 commit，每个 commit 独立可回滚。本任务不引入 CSS 变量、不动布局、不动字号、不动圆角、不动动效时长。

**Tech Stack:** 原生 CSS、`Edit` 工具、浏览器目视回归（无构建工具，无测试框架——CSS 主题不适合 TDD，验证手段为 grep 旧色残留 + 浏览器目视）。

**验证策略说明：**
- 每个 task 改完后用 `grep` 检查目标色值是否完全替换、旧色是否还残留在该 § 内
- 全部 task 做完后启动浏览器（用户操作 `open index.html`）逐屏目视，对照 spec § 4 验证清单

---

## File Structure

**唯一修改文件：** `theme.css`（共 290 行，按 §1–§12 分块）

**新建文件：** 无

**不动文件：**
- `index.html`（upstream 边界，fork 不动）
- `README.md`、`docs/**/*`（除本计划已写入的 spec 与 plan 文档外）

---

## Task 1: § 1 Base & Typography（页面底色 + 主文字）

**Files:**
- Modify: `theme.css:8-17`（§ 1 Base & Typography）

- [ ] **Step 1: 替换 body 背景与主文字色**

`theme.css:9-10` 当前：
```css
body {
  background: #F8FAFC;
  color: #0F172A;
```
改为：
```css
body {
  background: #FAFAFA;
  color: #09090B;
```

Edit 操作：
- old_string: `  background: #F8FAFC;\n  color: #0F172A;`
- new_string: `  background: #FAFAFA;\n  color: #09090B;`

- [ ] **Step 2: 验证 § 1 内无旧色残留**

Run:
```bash
sed -n '8,17p' theme.css | grep -E '#F8FAFC|#0F172A'
```
Expected: 无输出（旧色已全部移除）

Run:
```bash
sed -n '8,17p' theme.css | grep -E '#FAFAFA|#09090B'
```
Expected: 两行命中

- [ ] **Step 3: Commit**

```bash
git add theme.css
git commit -m "style: theme color § 1 base → minimal-business [fork-only]"
```

---

## Task 2: § 2 Header（黑底白字）

**Files:**
- Modify: `theme.css:19-32`（§ 2 Header）

- [ ] **Step 1: 替换 .hdr 背景色**

`theme.css:21` 当前：
```css
  background: #1E3A5F;
```
改为：
```css
  background: #18181B;
```

Edit 操作：
- old_string: `.hdr {\n  background: #1E3A5F;\n  padding: 20px 24px;\n  box-shadow: 0 1px 3px rgba(15,23,42,.08);\n}`
- new_string: `.hdr {\n  background: #18181B;\n  padding: 20px 24px;\n  box-shadow: 0 1px 3px rgba(15,23,42,.08);\n}`

注：box-shadow 与 padding 按 spec § 2 决定保留。

- [ ] **Step 2: 验证**

Run:
```bash
sed -n '19,32p' theme.css | grep -E '#1E3A5F'
```
Expected: 无输出

Run:
```bash
sed -n '19,32p' theme.css | grep -E '#18181B'
```
Expected: 1 行命中

- [ ] **Step 3: Commit**

```bash
git add theme.css
git commit -m "style: theme color § 2 header → minimal-business [fork-only]"
```

---

## Task 3: § 3 Top Storage Toolbar（保存按钮黑化）

**Files:**
- Modify: `theme.css:34-68`（§ 3 Top Storage Toolbar）

- [ ] **Step 1: 替换工具栏色彩**

`theme.css:34-68` 整体替换。当前：
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

改为：
```css
/* === 3. Top Storage Toolbar (override inline style) === */
body > div[style*="border-bottom:2px solid #edf2ff"] {
  background: #FFFFFF !important;
  border-bottom: 1px solid #E4E4E7 !important;
  padding: 12px 24px !important;
  box-shadow: none !important;
}
body > div[style*="border-bottom:2px solid #edf2ff"] > span {
  font-size: 14px !important;
  color: #3F3F46 !important;
}
body > div[style*="border-bottom:2px solid #edf2ff"] button {
  font-size: 14px !important;
  padding: 10px 18px !important;
  border-radius: 6px !important;
  transition: background 200ms ease !important;
}
body > div[style*="border-bottom:2px solid #edf2ff"] button[onclick*="saveData"] {
  background: #18181B !important;
  color: #FFFFFF !important;
}
body > div[style*="border-bottom:2px solid #edf2ff"] button[onclick*="loadData"] {
  background: #FFFFFF !important;
  color: #3F3F46 !important;
  border: 1.5px solid #E4E4E7 !important;
}
body > div[style*="border-bottom:2px solid #edf2ff"] button[onclick*="clearData"] {
  background: #FEF2F2 !important;
  color: #DC2626 !important;
  border: 1.5px solid #FECACA !important;
}
body > div[style*="border-bottom:2px solid #edf2ff"] #save-status {
  font-size: 12px !important;
  color: #16A34A !important;
}
```

变更点：
- `#E4E7EB` → `#E4E4E7`（toolbar 下边框）
- `#334155` → `#3F3F46`（span 文字、loadData 按钮文字）
- `#1E3A5F` → `#18181B`（saveData 按钮背景）
- `#CBD5E1` → `#E4E4E7`（loadData 按钮边）
- `#059669` → `#16A34A`（save-status 成功色）
- 清空按钮三色（`#FEF2F2 / #DC2626 / #FECACA`）保持

- [ ] **Step 2: 验证**

Run:
```bash
sed -n '34,68p' theme.css | grep -E '#1E3A5F|#334155|#CBD5E1|#059669|#E4E7EB'
```
Expected: 无输出

Run:
```bash
sed -n '34,68p' theme.css | grep -cE '#18181B|#3F3F46|#E4E4E7|#16A34A'
```
Expected: 至少 5（5 个新色值合计出现 ≥ 5 次）

- [ ] **Step 3: Commit**

```bash
git add theme.css
git commit -m "style: theme color § 3 toolbar → minimal-business [fork-only]"
```

---

## Task 4: § 4 Progress Bar（步骤条）

**Files:**
- Modify: `theme.css:70-91`（§ 4 Progress Bar）

- [ ] **Step 1: 替换 dot/ln/slabel 色值**

替换以下 5 行：

old_string:
```css
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

new_string:
```css
.dot.active { background: #18181B; }
.dot.done   { background: #16A34A; }
.dot.idle   { background: #E4E4E7; color: #71717A; }
.ln.done    { background: #16A34A; }
.ln.idle    { background: #E4E4E7; }
.slabel {
  font-size: 12px;
  color: #71717A;
  margin-left: 8px;
}
```

- [ ] **Step 2: 验证**

Run:
```bash
sed -n '70,91p' theme.css | grep -E '#1E3A5F|#059669|#E4E7EB|#64748B'
```
Expected: 无输出

- [ ] **Step 3: Commit**

```bash
git add theme.css
git commit -m "style: theme color § 4 progress → minimal-business [fork-only]"
```

---

## Task 5: § 5 Cards & Sections（卡片标题与分隔）

**Files:**
- Modify: `theme.css:93-122`（§ 5 Cards & Sections）

- [ ] **Step 1: 替换 .ctitle / .cbadge / .divider 色值**

old_string:
```css
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
```

new_string:
```css
.ctitle {
  font-size: 18px;
  color: #18181B;
  border-bottom: 2px solid #F4F4F5;
  padding-bottom: 12px;
  margin-bottom: 16px;
}
.cbadge {
  font-size: 12px;
  background: #F4F4F5;
  color: #18181B;
  padding: 2px 8px;
}
.divider {
  font-size: 14px;
  color: #3F3F46;
  padding: 16px 0 12px;
  border-top: 1px dashed #E4E4E7;
}
```

注：`.card`、`.nav`、`.grid`、`.fg` 不动（仅圆角/间距/阴影规格，无色值）。

- [ ] **Step 2: 验证**

Run:
```bash
sed -n '93,122p' theme.css | grep -E '#1E3A5F|#EEF2FF|#475569|#E4E7EB'
```
Expected: 无输出

- [ ] **Step 3: Commit**

```bash
git add theme.css
git commit -m "style: theme color § 5 cards → minimal-business [fork-only]"
```

---

## Task 6: § 6 Form Fields（输入框 + 蓝焦点）

**Files:**
- Modify: `theme.css:124-162`（§ 6 Form Fields）

- [ ] **Step 1: 替换表单全部色值**

old_string:
```css
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
```

new_string:
```css
label {
  font-size: 14px;
  color: #3F3F46;
}
.fn {
  font-size: 12px;
  background: #F4F4F5;
  color: #18181B;
  padding: 1px 6px;
}
.hint {
  font-size: 12px;
  line-height: 1.45;
  color: #71717A;
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
  border: 1.5px solid #E4E4E7;
  border-radius: 6px;
  background: #FFFFFF;
  color: #09090B;
  transition: border-color 200ms ease, box-shadow 200ms ease;
}
input:focus,
select:focus,
textarea:focus {
  outline: none;
  border-color: #2563EB;
  box-shadow: 0 0 0 3px rgba(37,99,235,.15);
  background: #FFFFFF;
}
```

注 `textarea { min-height: 72px; }` 这行（在原文件 § 6 末尾）不动。

- [ ] **Step 2: 验证**

Run:
```bash
sed -n '124,162p' theme.css | grep -E '#334155|#EEF2FF|#1E3A5F|#64748B|#CBD5E1|#0F172A|rgba\(30,58,95'
```
Expected: 无输出

Run:
```bash
sed -n '124,162p' theme.css | grep -E '#2563EB|rgba\(37,99,235'
```
Expected: 各 1 行命中（焦点色）

- [ ] **Step 3: Commit**

```bash
git add theme.css
git commit -m "style: theme color § 6 form + blue focus → minimal-business [fork-only]"
```

---

## Task 7: § 7 Radios / Checks

**Files:**
- Modify: `theme.css:164-181`（§ 7 Radios / Checks）

- [ ] **Step 1: 替换 .ri / .ci 色值**

old_string:
```css
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

new_string:
```css
.ri, .ci {
  font-size: 14px;
  padding: 9px 14px;
  border-radius: 6px;
  border: 1.5px solid #E4E4E7;
  background: #FFFFFF;
  transition: border-color 200ms ease, background 200ms ease;
}
.ri:hover, .ci:hover {
  border-color: #18181B;
  background: #FAFAFA;
}
.ri input, .ci input {
  accent-color: #18181B;
  width: 14px;
  height: 14px;
}
```

- [ ] **Step 2: 验证**

Run:
```bash
sed -n '164,181p' theme.css | grep -E '#CBD5E1|#1E3A5F|#EEF2FF'
```
Expected: 无输出

- [ ] **Step 3: Commit**

```bash
git add theme.css
git commit -m "style: theme color § 7 radios → minimal-business [fork-only]"
```

---

## Task 8: § 8 Buttons（CTA 与下载按钮全部黑化）

**Files:**
- Modify: `theme.css:183-213`（§ 8 Buttons）

- [ ] **Step 1: 替换全部按钮色值**

old_string:
```css
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

new_string:
```css
.btn {
  font-size: 15px;
  padding: 11px 20px;
  border-radius: 6px;
  transition: background 200ms ease, border-color 200ms ease, color 200ms ease;
}
.btn-p {
  background: #18181B;
  color: #FFFFFF;
}
.btn-p:hover { background: #27272A; }
.btn-s {
  background: #FFFFFF;
  color: #3F3F46;
  border: 1.5px solid #E4E4E7;
}
.btn-s:hover {
  background: #FAFAFA;
  border-color: #18181B;
  color: #18181B;
}
.btn-dl {
  font-size: 16px;
  background: #18181B;
  color: #FFFFFF;
  border-radius: 8px;
  padding: 12px 26px;
}
.btn-dl:hover { background: #27272A; }
.btn-dl:disabled { opacity: .5; cursor: not-allowed; }
```

变更点：
- `.btn-p` 主色：`#1E3A5F` → `#18181B`，hover `#15304D` → `#27272A`
- `.btn-s` 次按钮：`#334155` 文字 → `#3F3F46`，`#CBD5E1` 边 → `#E4E4E7`，hover 蓝紫底 → `#FAFAFA`，hover 蓝边/字 → `#18181B`
- `.btn-dl` 下载按钮：`#059669` → `#18181B`，hover `#047857` → `#27272A`

- [ ] **Step 2: 验证**

Run:
```bash
sed -n '183,213p' theme.css | grep -E '#1E3A5F|#15304D|#334155|#CBD5E1|#EEF2FF|#059669|#047857'
```
Expected: 无输出

- [ ] **Step 3: Commit**

```bash
git add theme.css
git commit -m "style: theme color § 8 buttons (incl. .btn-dl) → minimal-business [fork-only]"
```

---

## Task 9: § 9 Country Cards（蓝 ring 区分 selected）

**Files:**
- Modify: `theme.css:215-252`（§ 9 Country Cards）

- [ ] **Step 1: 替换国家卡全部状态色**

old_string:
```css
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

new_string:
```css
.country-card {
  padding: 14px 12px;
  border-radius: 8px;
  border: 2px solid #E4E4E7;
  transition: border-color 200ms ease, background 200ms ease, box-shadow 200ms ease;
}
.country-card:hover {
  border-color: #18181B;
  background: #FAFAFA;
  transform: none;
  box-shadow: 0 1px 3px rgba(15,23,42,.08);
}
.country-card.selected {
  border-color: #18181B;
  background: #FAFAFA;
  box-shadow: 0 0 0 3px rgba(37,99,235,.15);
}
.country-card.no-pdf:hover {
  border-color: #E4E4E7;
  background: #FFFFFF;
  transform: none;
  box-shadow: none;
}
.country-card.is-link {
  border-color: #D97706;
}
.country-card.is-link:hover {
  border-color: #B45309;
  background: #FFFBEB;
  transform: none;
  box-shadow: 0 1px 3px rgba(15,23,42,.08);
}
.country-name { font-size: 14px; }
.country-status { font-size: 12px; }
.status-ready { color: #16A34A; }
.status-pending { color: #D97706; }
.status-link { color: #B45309; }
```

变更点：
- 普通边、no-pdf hover 边：`#CBD5E1` → `#E4E4E7`
- hover 边、selected 边：`#1E3A5F` → `#18181B`
- hover 底、selected 底：`#EEF2FF` → `#FAFAFA`
- selected ring：`rgba(30,58,95,.18)` → `rgba(37,99,235,.15)`（蓝 ring，spec § 9 区分 hover/selected 的关键信号）
- is-link 普通边：`#F59E0B` → `#D97706`（每档加深一级，保持原层级）
- is-link hover 边：`#D97706` → `#B45309`
- `.status-ready`：`#059669` → `#16A34A`
- `.status-pending`：`#F59E0B` → `#D97706`
- `.status-link`：`#D97706` → `#B45309`

- [ ] **Step 2: 验证**

Run:
```bash
sed -n '215,252p' theme.css | grep -E '#CBD5E1|#1E3A5F|#EEF2FF|rgba\(30,58,95|#F59E0B|#059669'
```
Expected: 无输出

Run:
```bash
sed -n '215,252p' theme.css | grep -E 'rgba\(37,99,235'
```
Expected: 1 行命中（selected 蓝 ring）

- [ ] **Step 3: Commit**

```bash
git add theme.css
git commit -m "style: theme color § 9 country cards + blue ring → minimal-business [fork-only]"
```

---

## Task 10: § 10 Sumbox（中性灰汇总框）

**Files:**
- Modify: `theme.css:254-270`（§ 10 Sumbox / Tip）

- [ ] **Step 1: 替换 sumbox 全部色值**

old_string:
```css
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

new_string:
```css
.sumbox {
  background: #FAFAFA;
  border: 1.5px solid #E4E4E7;
  border-radius: 8px;
}
.sumbox h3 {
  font-size: 13px;
  color: #18181B;
}
.sumlbl { color: #3F3F46; }
.sumval { color: #18181B; }
.sumrow { font-size: 12px; }
.tip {
  font-size: 12px;
  border-radius: 6px;
}
```

变更点（spec § 10 「从庆祝完成 → 信息汇总」）：
- 背景：浅绿 `#F0FDF4` → `#FAFAFA`
- 边框：`#10B981` → `#E4E4E7`
- h3 标题：`#065F46` → `#18181B`
- 标签：`#065F46` → `#3F3F46`
- 数值：`#047857` → `#18181B`

- [ ] **Step 2: 验证**

Run:
```bash
sed -n '254,270p' theme.css | grep -E '#F0FDF4|#10B981|#065F46|#047857'
```
Expected: 无输出

- [ ] **Step 3: Commit**

```bash
git add theme.css
git commit -m "style: theme color § 10 sumbox → minimal-business [fork-only]"
```

---

## Task 11: 全局残留扫描

**Files:**
- Read-only: `theme.css`

确认整个 `theme.css` 中所有旧政府文书风色值已彻底清除。

- [ ] **Step 1: 扫描旧色值是否仍残留**

Run:
```bash
grep -nE '#1E3A5F|#15304D|#EEF2FF|#CBD5E1|#334155|#64748B|#475569|#0F172A|#F8FAFC|#E4E7EB|#059669|#047857|#10B981|#065F46|#F0FDF4|#F59E0B|rgba\(30,58,95' theme.css
```
Expected: 无输出（所有旧色全部消失）

如有命中：定位到对应 § 后回到该 task 修复，再次跑此命令直至无输出。

- [ ] **Step 2: 扫描新色值确实写入**

Run:
```bash
grep -cE '#18181B' theme.css
```
Expected: ≥ 8（Header、CTA、btn-s hover、btn-dl、ctitle、cbadge、country hover、country selected、sumbox h3 等多处）

Run:
```bash
grep -cE '#2563EB' theme.css
```
Expected: 1（仅 input focus 边框）

Run:
```bash
grep -cE 'rgba\(37,99,235' theme.css
```
Expected: 2（input focus ring + country selected ring）

Run:
```bash
grep -cE '#16A34A' theme.css
```
Expected: ≥ 4（save-status、dot.done、ln.done、status-ready）

- [ ] **Step 3: 无需 commit（仅扫描，未改文件）**

如果 Step 1–2 全部通过，进入 Task 12。

---

## Task 12: 浏览器目视回归

**Files:**
- Read-only: `index.html`、`theme.css`

按 spec § 4「验证清单」逐屏目视。本步骤需要用户配合（在浏览器里点击）。

- [ ] **Step 1: 启动页面**

Run:
```bash
open index.html
```

页面应在默认浏览器打开。**如果用户在 SSH 或无桌面环境，跳过此 Task 并标记为「已通过 grep 扫描，待用户本地验证」。**

- [ ] **Step 2: 逐项核对 spec § 4 清单**

人工目视，对照 [`2026-05-15-minimal-business-theme-design.md`](../specs/2026-05-15-minimal-business-theme-design.md) § 4：

- [ ] Header 黑底白字，无浅蓝残留
- [ ] 工具栏「保存」黑色、「载入」白底、「清空」红字
- [ ] 步骤条：当前=黑、已完成=绿、未达=灰
- [ ] input 聚焦时蓝边 + 蓝 ring（页面唯一的蓝）
- [ ] 卡片标题下划线浅灰，不是蓝紫
- [ ] Radio/Checkbox hover 与选中浅灰底，不是蓝紫；圆点黑
- [ ] 主 CTA「下一步」黑色；下载按钮也黑（不再绿）
- [ ] 国家卡 selected 外圈淡蓝 ring
- [ ] is-link 国家卡边框为 amber-600，不刺眼
- [ ] Sumbox 中性灰，不再浅绿
- [ ] 必填星号、错误红、清空按钮红色保持
- [ ] 移动端（DevTools 调到 ≤580px）无回归
- [ ] 桌面端（≥768px）无回归
- [ ] DevTools 模拟 `prefers-reduced-motion: reduce` 仍生效

如有任一项不符 spec，回到对应 § Task 修补再 commit。

- [ ] **Step 3: 收尾 commit（可选）**

仅在目视回归过程中发现并修补了内容时执行：

```bash
git add theme.css
git commit -m "style: theme color regression fixes [fork-only]"
```

如目视全部通过、无修改，**跳过本 Step**。

---

## Self-Review

**1. Spec coverage:**

| Spec 章节 | 覆盖 task |
|---|---|
| § 1 色板（13 token） | Task 1–10 整体覆盖 |
| § 2.1 Base | Task 1 |
| § 2.2 Header | Task 2 |
| § 2.3 Toolbar | Task 3 |
| § 2.4 Progress Bar | Task 4 |
| § 2.5 Cards | Task 5 |
| § 2.6 Form Fields | Task 6 |
| § 2.7 Radios/Checks | Task 7 |
| § 2.8 Buttons（含 .btn-dl 黑化） | Task 8 |
| § 2.9 Country Cards | Task 9 |
| § 2.10 Sumbox | Task 10 |
| § 2.11–12 Responsive & Reduced Motion | 不动（spec 明确） |
| § 3 设计原则 | 整个 plan 遵守（不引入 CSS 变量、不动 upstream、不动布局） |
| § 4 验证清单 | Task 12 逐项核对 |
| § 5 提交节奏 | 每 task 一个 commit，message 模板与 fork 历史一致 |

无遗漏。

**2. Placeholder scan:** 已完整复述每段新 CSS，没有 TBD/TODO/「同上」。每个验证步都给了具体 grep 命令与期望输出。

**3. 类型/命名一致性:**
- `#18181B` 在 Task 2/3/4/5/6/7/8/9/10 中含义一致（zinc-900 主品牌色）
- `#2563EB` 仅在 Task 6（input focus）使用 1 次；selected ring 用对应的 `rgba(37,99,235,.15)`
- `#16A34A` 在 Task 3/4/9 中含义一致（绿，状态语义）
- `#D97706` 在 Task 9 既是 is-link 普通边，也是 status-pending 字色（spec § 1 也将它归为「警告橙」）
- `#FAFAFA` 既是页面底（Task 1）也是 hover/selected 卡片底（Task 7/9）和 sumbox 底（Task 10），是 spec 的有意为之（中性灰统一）
- 验证 grep 命令的色值与每个 task 的 new_string 内容一致

无歧义。
