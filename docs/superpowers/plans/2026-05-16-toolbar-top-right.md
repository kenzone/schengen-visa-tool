# 顶部存档工具栏右上化实施计划

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development to implement this plan task-by-task.

**Goal:** 把顶部存档工具栏从 Header 下方独立白条迁移到 Header 内部右上角，配色改外交风金/白/红透明小按钮，落实 spec [`2026-05-16-toolbar-top-right-design.md`](../specs/2026-05-16-toolbar-top-right-design.md)。

**Architecture:** 在 `theme.css` 末尾追加新区段 `§ 14. Toolbar Header Integration`（紧接 § 13 之后），用 CSS 后置覆盖前层 § 3 的 toolbar 规则。零修改 § 1–13 现有内容。3 个 commit：A 容器 + B 按钮 + C 移动端。

**Tech Stack:** 原生 CSS（absolute 定位、`:not()` 选择器、属性选择器、@media）。

---

## File Structure

**唯一修改文件：** `theme.css`（仅末尾追加新区段 § 14）

---

## Task 1: A · Toolbar 容器脱离独立条 + 隐藏标签

**Files:**
- Modify: `theme.css` 末尾（在 § 13.E `.sumbox::before` 之后追加 § 14 标头 + 14.A）

- [ ] **Step 1: 追加 § 14 区段标头与 14.A 规则**

old_string（§ 13.E `.sumbox::before` 块作为锚点，复制到完整结尾的 `}`）:
```css
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

new_string:
```css
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

/* === 14. Toolbar Header Integration ============ */

/* 14.A Toolbar floats to header top-right + hide label */
body > div[style*="border-bottom:2px solid #edf2ff"] {
  position: absolute !important;
  top: 14px !important;
  right: 20px !important;
  background: transparent !important;
  border-bottom: none !important;
  padding: 0 !important;
  box-shadow: none !important;
  display: flex !important;
  align-items: center !important;
  flex-wrap: nowrap !important;
  gap: 6px !important;
  z-index: 10;
}
body > div[style*="border-bottom:2px solid #edf2ff"] > span:not(#save-status) {
  display: none !important;
}
.hdr {
  padding-right: 280px !important;
}
```

- [ ] **Step 2: 验证**

Run:
```bash
grep -nE '14\. Toolbar Header Integration|14\.A Toolbar floats' theme.css
```
Expected: 2 行命中

Run:
```bash
grep -nE 'position: absolute !important' theme.css | wc -l
```
Expected: ≥ 1（14.A 中 1 处；前层 § 13 中也可能有 `.country-card.selected::after` `.sumbox::before` 等使用 `position: absolute`，但那些不带 `!important`；本 grep 期望命中数随实施而定，主要确认 14.A 规则存在）

Run:
```bash
grep -cE 'padding-right: 280px !important' theme.css
```
Expected: 1

- [ ] **Step 3: Commit**

```bash
git add theme.css
git commit -m "style: toolbar A · float to header top-right [fork-only]"
```

---

## Task 2: B · 三种按钮外交配色 + save-status 衬线

**Files:**
- Modify: `theme.css` 末尾（在 14.A 之后追加 14.B）

- [ ] **Step 1: 追加 14.B 规则**

old_string（14.A 末尾 `.hdr` 规则作为锚点）:
```css
.hdr {
  padding-right: 280px !important;
}
```

⚠️ 注意：theme.css 中 `.hdr {` 出现多次（§ 2 和 § 13.A 都有）。但 `padding-right: 280px !important` 只在新追加的 14.A 中存在，所以 old_string 唯一。

new_string:
```css
.hdr {
  padding-right: 280px !important;
}

/* 14.B Diplomatic button colors */
body > div[style*="border-bottom:2px solid #edf2ff"] button {
  font-size: 11px !important;
  padding: 5px 10px !important;
  border-radius: 3px !important;
  font-family: 'Songti SC', 'STSong', SimSun, Georgia, serif !important;
  letter-spacing: 0.04em !important;
  font-weight: 500 !important;
}
body > div[style*="border-bottom:2px solid #edf2ff"] button[onclick*="saveData"] {
  background: transparent !important;
  color: #C9A961 !important;
  border: 1px solid #B8860B !important;
}
body > div[style*="border-bottom:2px solid #edf2ff"] button[onclick*="saveData"]:hover {
  background: #B8860B !important;
  color: #18181B !important;
}
body > div[style*="border-bottom:2px solid #edf2ff"] button[onclick*="loadData"] {
  background: transparent !important;
  color: rgba(255,255,255,.75) !important;
  border: 1px solid rgba(255,255,255,.25) !important;
}
body > div[style*="border-bottom:2px solid #edf2ff"] button[onclick*="loadData"]:hover {
  background: rgba(255,255,255,.08) !important;
  color: #FFFFFF !important;
  border-color: rgba(255,255,255,.5) !important;
}
body > div[style*="border-bottom:2px solid #edf2ff"] button[onclick*="clearData"] {
  background: transparent !important;
  color: #FCA5A5 !important;
  border: 1px solid rgba(252,165,165,.35) !important;
}
body > div[style*="border-bottom:2px solid #edf2ff"] button[onclick*="clearData"]:hover {
  background: rgba(252,165,165,.10) !important;
  color: #FECACA !important;
  border-color: rgba(252,165,165,.6) !important;
}
body > div[style*="border-bottom:2px solid #edf2ff"] #save-status {
  font-size: 10px !important;
  color: #C9A961 !important;
  font-style: italic !important;
  font-family: 'Songti SC', 'STSong', SimSun, Georgia, serif !important;
  letter-spacing: 0.05em !important;
}
```

- [ ] **Step 2: 验证**

Run:
```bash
grep -nE '14\.B Diplomatic button colors' theme.css
```
Expected: 1 行命中

Run:
```bash
grep -cE 'rgba\(255,255,255,\.75\)|rgba\(252,165,165,\.35\)' theme.css
```
Expected: 2（loadData 半透明白字 + clearData 半透明红边）

Run:
```bash
grep -cE 'color: #C9A961 !important' theme.css
```
Expected: 2（saveData 字 + save-status 字；其他 `#C9A961` 出现在 § 13 但不是这条特定规则）

- [ ] **Step 3: Commit**

```bash
git add theme.css
git commit -m "style: toolbar B · diplomatic button colors [fork-only]"
```

---

## Task 3: C · 移动端响应式回退

**Files:**
- Modify: `theme.css` 末尾（在 14.B 之后追加 14.C）

- [ ] **Step 1: 追加 14.C 规则**

old_string（14.B 末尾 #save-status 规则作为锚点）:
```css
body > div[style*="border-bottom:2px solid #edf2ff"] #save-status {
  font-size: 10px !important;
  color: #C9A961 !important;
  font-style: italic !important;
  font-family: 'Songti SC', 'STSong', SimSun, Georgia, serif !important;
  letter-spacing: 0.05em !important;
}
```

new_string:
```css
body > div[style*="border-bottom:2px solid #edf2ff"] #save-status {
  font-size: 10px !important;
  color: #C9A961 !important;
  font-style: italic !important;
  font-family: 'Songti SC', 'STSong', SimSun, Georgia, serif !important;
  letter-spacing: 0.05em !important;
}

/* 14.C Mobile responsive fallback */
@media (max-width: 580px) {
  body > div[style*="border-bottom:2px solid #edf2ff"] {
    position: static !important;
    top: auto !important;
    right: auto !important;
    background: #1F1F23 !important;
    padding: 8px 16px !important;
    flex-wrap: wrap !important;
    border-bottom: 1px solid rgba(184,134,11,.3) !important;
    z-index: auto;
  }
  .hdr {
    padding-right: 16px !important;
  }
}
```

- [ ] **Step 2: 验证**

Run:
```bash
grep -nE '14\.C Mobile responsive fallback' theme.css
```
Expected: 1 行命中

Run:
```bash
grep -cE 'position: static !important' theme.css
```
Expected: 1（14.C 内 toolbar 移动端 fallback）

Run:
```bash
grep -cE '#1F1F23' theme.css
```
Expected: 1（移动端 toolbar 微亮黑底）

- [ ] **Step 3: Commit**

```bash
git add theme.css
git commit -m "style: toolbar C · mobile responsive fallback [fork-only]"
```

---

## Task 4: 浏览器目视回归（用户执行）

**用户操作。** 不在 implementer subagent 范围。

按 spec § 4 验证清单逐项目视：

- 桌面端：toolbar 浮于 Header 右上角；3 个按钮金/白/红透明配色；hover 状态正确；标签隐藏；save-status 浅金 italic
- 滚动测试：toolbar 跟随 Header 滚出，不浮顶
- 移动端（DevTools 模拟 375px）：toolbar 回到 Header 下方深底条，按钮可换行

---

## Self-Review

**1. Spec coverage:**
- spec § 2.A → Task 1（容器 + 隐藏标签 + Header padding-right）
- spec § 2.B → Task 2（按钮配色 + save-status）
- spec § 2.C → Task 3（移动端响应式）
- spec § 4 验证清单 → Task 4（用户目视）
- spec § 5 提交节奏（3 commit）→ Task 1-3 各 1 commit

无遗漏。

**2. Placeholder scan:** 每个 Edit 都给出完整 old_string / new_string；验证 grep 都给出确切命令与期望输出；无 TBD。

**3. Edit 顺序依赖:**
- Task 2 的 old_string 是 Task 1 末尾的 `.hdr { padding-right: 280px !important; }`，要在 Task 1 完成后才能匹配——这是预期行为
- Task 3 的 old_string 是 Task 2 末尾的 `#save-status` 规则，同理
- Edit 工具按 string match，每次追加不影响前面已存在的内容

**4. CSS 特异性 / 覆盖正确性:**
- 14.A 的 toolbar 容器选择器与 § 3 前层规则同特异性，14.A 在文件后面 → 后定义胜出 ✓
- 14.B 的 saveData/loadData/clearData 按钮选择器与 § 3 前层一致，14.B 后定义胜出 ✓
- `:not(#save-status)` 选择器特异性 = `(0,1,1)`（class + 属性）+ `:not(id)` 内部 = `(1,0,0)`，最终 `(1,1,1)`——不影响 #save-status，因为后者特异性 `(1,0,0)` + `body > div[...]` 复合选择器仍高
- 14.C 的 `@media` 内规则与 14.A 同选择器但更后定义且窗口匹配时生效 ✓

**5. 风险控制:**
- 280px Header padding-right 是估算值，可能在某些视口下不够——验证清单已要求"目视检查 Header 内容不与 toolbar 重叠"
- `:not(#save-status)` 假设 toolbar 内只有 1 个非 status span，依赖当前 HTML（line 103 是「💾 数据存储：」）；upstream 不变前安全

无歧义。
