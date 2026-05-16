# 顶部存档工具栏融入 Header 右上角设计稿

**日期：** 2026-05-16
**作用对象：** `theme.css`（fork-only 主题层）
**前置：**
- [[2026-05-15-minimal-business-theme-design]]（色彩底层）
- [[2026-05-16-restrained-refinement-design]]（精修层）
- [[2026-05-16-diplomatic-passport-design]]（外交护照层）

## 0. 背景与目标

当前顶部存档工具栏（保存/加载/清空 + save-status）作为 Header 下方的独立白底横条，视觉上"打断"了护照黑底 Header 的整体性。本稿把工具栏脱离独立条，绝对定位到 Header 内部右上角，配色改为外交风金/白/红透明小按钮，让工具栏成为 Header 一部分。

**记忆点：** 工具栏重新变成"官方护照"的一部分。

**叠加而非新增 § 编号：** 修改现有 § 3（Top Storage Toolbar inline override）的部分规则；不在 § 13 之后新增编号区段。

**用户决策（已拍板）：**
1. 隐藏「💾 数据存储：」标签（按钮文字已自带语义）
2. 桌面 Header `padding-right: 280px` 给 toolbar 让位

## 1. 修改范围

仅修改 `theme.css` 现有 § 3 区段中的规则；新增 § 11 移动端响应式分支。

不动：
- `theme.css` § 1, § 2, § 4-12, § 13 全部规则
- HTML 结构与上游 inline style
- 现有色彩 token、字体栈

## 2. 三组改动

### A. Toolbar 容器脱离独立条

```css
/* 旧 § 3 容器规则覆盖 → 新规则 */
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

/* 隐藏「💾 数据存储：」标签 */
body > div[style*="border-bottom:2px solid #edf2ff"] > span:not(#save-status) {
  display: none !important;
}

/* Header 给 toolbar 让位 */
.hdr {
  padding-right: 280px !important;
}
```

**说明：**
- `position: absolute` 相对最近的 `position: relative` 祖先——`<body>` 默认 static，但本工具栏 absolute 后浮于 viewport 顶部右侧（top: 14px, right: 20px），刚好落在 Header 内部右上角的位置。
- 不改用 `position: fixed`，因为：fixed 会让工具栏在滚动时一直浮顶部，但用户填表时已经离开 Header 视野，工具栏继续浮在屏幕右上反而干扰阅读。absolute 跟随 Header 滚出更合理。
- z-index 10 确保浮于 Header 装饰元素（::before 金线、::after 文字）之上。
- `flex-wrap: nowrap`（覆盖原 inline `flex-wrap: wrap`）防止按钮换行。
- `:not(#save-status)` 仅隐藏装饰标签，不影响动态状态文字。

### B. 三种按钮外交配色

```css
/* 按钮统一缩小 + 衬线 */
body > div[style*="border-bottom:2px solid #edf2ff"] button {
  font-size: 11px !important;
  padding: 5px 10px !important;
  border-radius: 3px !important;
  font-family: 'Songti SC', 'STSong', SimSun, Georgia, serif !important;
  letter-spacing: 0.04em !important;
  font-weight: 500 !important;
}

/* 保存（主）：透明底 + 暗金边 + 暖金字 */
body > div[style*="border-bottom:2px solid #edf2ff"] button[onclick*="saveData"] {
  background: transparent !important;
  color: #C9A961 !important;
  border: 1px solid #B8860B !important;
}
body > div[style*="border-bottom:2px solid #edf2ff"] button[onclick*="saveData"]:hover {
  background: #B8860B !important;
  color: #18181B !important;
}

/* 加载（次）：透明底 + 半透明白边 + 半透明白字 */
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

/* 清空（危险）：透明底 + 半透明红边 + 浅红字 */
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

/* save-status: 浅金 italic 衬线 */
body > div[style*="border-bottom:2px solid #edf2ff"] #save-status {
  font-size: 10px !important;
  color: #C9A961 !important;
  font-style: italic !important;
  font-family: 'Songti SC', 'STSong', SimSun, Georgia, serif !important;
  letter-spacing: 0.05em !important;
}
```

### C. 移动端响应式回退

```css
/* § 11 移动端追加：toolbar 不再 absolute */
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
  /* 移动端 Header 不再 padding-right */
  .hdr {
    padding-right: 16px !important;  /* 与现有 § 11 .hdr padding 协调 */
  }
}
```

**说明：**
- 移动端 ≤580px：toolbar 回到 Header 下方独立条，但保持外交风深底（`#1F1F23` 微亮黑）+ 暗金细线
- `flex-wrap: wrap` 允许按钮换行（窄屏可能放不下 3 按钮 + status）
- Header 的 padding-right 在移动端被 § 11 中既有 `.hdr { padding: 16px }` 覆盖
- 实际上现有 § 11 设的 `.hdr { padding: 16px }` 是 shorthand 会覆盖所有四边——所以本规则的 `padding-right: 16px !important` 是与之保持一致

## 3. 已知风险与对策

1. **如果 Header 副标题在某些状态下比预设长** → 可能跟 toolbar 视觉接触。对策：spec § 4 验证清单包含"目视检查 Header 内容不与 toolbar 重叠"。如果重叠，可以在 spec 后续 patch 把 padding-right 从 280px 调到 320px。

2. **`:not(#save-status)` 选择器在工具栏内部隐藏第一个 span** → 依赖工具栏只有 1 个非 status span。当前 HTML 第 103 行只有「💾 数据存储：」一个非 status span，符合假设。

3. **absolute 定位脱离文档流后，Header 高度由 Header 内部内容决定** → 当 Header 内容（标题文字 + EU 圆章）很短时，Header 仍然有最低高度（按当前 § 13.A 设定的 `padding-bottom: 28px` + 内部内容）；toolbar 的 `top: 14px` + 按钮 ~32px 高度 = 终点 46px，应该落在 Header 区域内（Header 内部 .eu 圆章是 52px 高，所以 Header 至少 52 + 20 (padding) + 28 (padding-bottom) ≈ 100px 高，46px 终点在 Header 内）。

## 4. 验证清单

实施完成后人工目视：

- [ ] Header 下方不再有独立白色 toolbar 条
- [ ] Header 右上角浮着 3 个小按钮（保存=金边、加载=白边、清空=红边）+ 后面跟 save-status
- [ ] 不可见「💾 数据存储：」标签
- [ ] 鼠标悬停保存按钮：金底黑字
- [ ] 鼠标悬停加载按钮：8% 白底 + 全白字
- [ ] 鼠标悬停清空按钮：10% 红底 + 红字
- [ ] save-status 显示时是浅金色 italic 衬线小字
- [ ] Header 内容（EU 圆章 + 标题文字）不被 toolbar 遮挡
- [ ] 滚动页面时 toolbar 跟随 Header 滚出（不浮顶）
- [ ] 移动端（≤580px）：toolbar 回到 Header 下方深底条，按钮可换行
- [ ] 移动端 toolbar 顶部 1px 暗金细线
- [ ] DevTools `prefers-reduced-motion: reduce`：toolbar 仍可见、按钮 hover 无过渡

## 5. 提交节奏

3 个 commit：
```
style: toolbar A · float to header top-right [fork-only]
style: toolbar B · diplomatic button colors [fork-only]
style: toolbar C · mobile responsive fallback [fork-only]
```
