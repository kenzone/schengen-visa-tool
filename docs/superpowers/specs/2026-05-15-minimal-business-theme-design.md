# 极简商务主题（Minimal Business Theme）设计稿

**日期：** 2026-05-15
**作用对象：** `theme.css`（fork-only 主题层，不动 upstream `index.html`）
**前置：** [[2026-05-15-government-theme-design]]（被本稿替换）

## 0. 背景与目标

当前 fork 已实现「政府文书风格」主题，主色为深海军蓝 `#1E3A5F`、强调绿 `#059669`。用户反馈希望整体气质更「商务专业」，方向定为 **极简灰黑 + 单色强调**（Linear / Stripe / shadcn 系），区别于政府文书的「权威严肃」。

**核心精神：** 主视觉只保留 **1 个品牌色 = 黑**。蓝、绿、红、橙仅承担「状态语义」，不参与品牌表达。颜色词汇表越短，设计语言越纯净。

**非目标：**
- 不改变布局、字号、行高、圆角、间距、阴影规格、响应式断点、reduced-motion
- 不动 upstream `index.html` 的 `<style>` 块；所有覆盖仍由 `theme.css` 收口
- 不引入新 CSS 文件、不引入 CSS 变量重构（保持现有结构，仅替换色值）

## 1. 色板（13 个 token）

| 语义 | 旧值 | 新值 | 说明 |
|---|---|---|---|
| 页面底 | `#F8FAFC` | `#FAFAFA` | 纸白 |
| 卡片面 | `#FFFFFF` | `#FFFFFF` | 不变 |
| 主文字 | `#0F172A` | `#09090B` | 接近纯黑 |
| 次文字 | `#334155` | `#3F3F46` | zinc-700 |
| 弱文字 | `#64748B` | `#71717A` | zinc-500 |
| 边框 | `#CBD5E1` | `#E4E4E7` | zinc-200，中性 |
| 浅分隔 | `#E4E7EB` | `#F4F4F5` | zinc-100 |
| **主色** | `#1E3A5F` | **`#18181B`** | zinc-900，CTA / Header / 标题 |
| 主色 hover | `#15304D` | `#27272A` | zinc-800 |
| **焦点蓝** | — | **`#2563EB`** | blue-600，**仅** 焦点态 |
| 焦点 ring | `rgba(30,58,95,.18)` | `rgba(37,99,235,.15)` | 蓝 ring |
| 成功绿 | `#059669` | `#16A34A` | green-600，**仅** 状态指示（步骤完成、save-status） |
| 警告橙 | `#F59E0B` | `#D97706` | amber-600，**仅** is-link 国家卡 |
| 错误红 | `#DC2626` | `#DC2626` | 不变 |

**色彩使用纪律：**
- **黑** 是唯一的「品牌色」，承担：Header 背景、主 CTA、标题、激活步骤、选中边框
- **蓝** 只出现在交互焦点：input/select/textarea focus 边框 + ring，国家卡 selected ring
- **绿** 只出现在状态：步骤完成 dot/线、保存成功提示、汇总框「✓ 合法」标签
- **橙** 只出现在 `country-card.is-link`（外链型国家）
- **红** 只出现在必填星号、清空按钮、错误提示

## 2. 组件层映射（按 theme.css 现有 § 编号）

### § 1 Base & Typography
- `body { background: #FAFAFA; color: #09090B; }`
- 字号、行高、字体栈不变

### § 2 Header
- `.hdr { background: #18181B; }`
- `.hdr` 的 `box-shadow` **保留**（避免与下方白底工具栏融为一团），但因 Header 是黑底，原阴影 `rgba(15,23,42,.08)` 在视觉上几乎不可见——保留以防屏幕反射条件下边界不清

### § 3 Top Storage Toolbar (inline override)
- 工具栏底色保持白
- 「保存」按钮：背景 `#18181B`、白字（旧深蓝换黑）
- 「载入」按钮：白底 + `#E4E4E7` 边 + `#3F3F46` 字
- 「清空」按钮：保留 `#FEF2F2` 底 + `#DC2626` 字 + `#FECACA` 边
- `#save-status`：`#16A34A`

### § 4 Progress Bar
- `.dot.active { background: #18181B; }`（黑）
- `.dot.done { background: #16A34A; }`（绿）
- `.dot.idle { background: #E4E4E7; color: #71717A; }`
- `.ln.done { background: #16A34A; }`
- `.ln.idle { background: #E4E4E7; }`
- `.slabel { color: #71717A; }`

### § 5 Cards & Sections
- `.ctitle { color: #18181B; border-bottom: 2px solid #F4F4F5; }`
- `.cbadge { background: #F4F4F5; color: #18181B; }`（淘汰浅蓝紫）
- `.divider { color: #3F3F46; border-top: 1px dashed #E4E4E7; }`
- 其它（圆角、阴影、padding、`.nav margin-top`、`.grid gap`、`.fg gap`）不动

### § 6 Form Fields
- `label { color: #3F3F46; }`
- `.fn { background: #F4F4F5; color: #18181B; }`
- `.hint { color: #71717A; }`
- `.req { color: #DC2626; }`（不变）
- `input/select/textarea { border: 1.5px solid #E4E4E7; color: #09090B; }`
- **focus 态（页面唯一彩色焦点）：**
  ```css
  border-color: #2563EB;
  box-shadow: 0 0 0 3px rgba(37,99,235,.15);
  ```

### § 7 Radios / Checks
- `.ri, .ci { border: 1.5px solid #E4E4E7; }`
- hover：`border-color: #18181B; background: #FAFAFA;`（灰，而非旧的浅蓝紫）
- `accent-color: #18181B;`（黑色 radio 圆点）

### § 8 Buttons
- `.btn-p { background: #18181B; color: #FFFFFF; }`，hover `#27272A`
- `.btn-s { background: #FFFFFF; color: #3F3F46; border: 1.5px solid #E4E4E7; }`
- `.btn-s:hover { background: #FAFAFA; border-color: #18181B; color: #18181B; }`
- **`.btn-dl { background: #18181B; }`，hover `#27272A`**（与主 CTA 统一，淘汰原 `#059669` 绿）

### § 9 Country Cards
- 普通态：`border: 2px solid #E4E4E7;`
- hover：`border-color: #18181B; background: #FAFAFA;`（去掉旧的浅蓝紫底）
- selected：`border-color: #18181B; background: #FAFAFA; box-shadow: 0 0 0 3px rgba(37,99,235,.15);`（蓝 ring 区分 hover 与 selected——这是 selected 态唯一的彩色信号）
- `country-card.no-pdf:hover` 仍维持「不可交互」灰态：`border-color: #E4E4E7; background: #FFFFFF;`
- `country-card.is-link { border-color: #D97706; }`，hover `border-color: #B45309; background: #FFFBEB;`
- `.status-ready { color: #16A34A; }`
- `.status-pending { color: #D97706; }`
- `.status-link { color: #B45309; }`

### § 10 Sumbox / Tip
旧版浅绿底 + 绿边「庆祝感」过强，新版改为中性灰：
- `.sumbox { background: #FAFAFA; border: 1.5px solid #E4E4E7; }`
- `.sumbox h3 { color: #18181B; }`
- `.sumlbl { color: #3F3F46; }`
- `.sumval { color: #18181B; }`
- 「✓ 合法」类成功标记仍可用 `#16A34A`（由 inline 文本/图标承担，不在 sumbox 整体外观中）
- `.tip` 仅圆角字号，不引入特殊色

### § 11–12 Responsive & Reduced Motion
**完全不动。**

## 3. 设计原则与边界

1. **不引入 CSS 变量重构。** 现有 `theme.css` 是直接色值；本次仅做 1:1 替换，保持 diff 可审计。CSS 变量化是另一个主题，不在本次范围。
2. **不动 upstream `index.html`。** 维持 fork-only 边界。
3. **不动布局/排版/动效时长。** 仅改色值与极少数边框宽度（保持原 `1.5px / 2px` 不变）。
4. **保留 200ms ease 过渡。** 极简风也需要平滑反馈，不要为了「克制」去掉 transition。
5. **黑色不"漂"。** 所有「黑」必须是 `#18181B`（zinc-900）而非 `#000000`。纯黑在 `#FAFAFA` 底色上对比度过强，会显廉价。
6. **可访问性。** 主文字 `#09090B` on `#FAFAFA` ≈ 19:1 对比度，远超 WCAG AAA；蓝焦点 `#2563EB` on `#FFFFFF` ≈ 5.17:1，达 AA。

## 4. 验证清单

实施完成后人工逐屏检查：

- [ ] Header 是黑底白字，无浅蓝残留
- [ ] 工具栏「保存」按钮黑色，「载入」白底，「清空」红字
- [ ] 步骤条：当前步骤黑点、已完成绿点、未达灰点
- [ ] 任意 input/select 聚焦时，边框与 ring 是蓝色（这是页面唯一的蓝）
- [ ] 卡片标题（如「§ 1 基本信息」）下划线是浅灰 `#F4F4F5`，不是蓝紫
- [ ] Radio/Checkbox hover 与选中态背景是浅灰，不是蓝紫；圆点本身是黑
- [ ] 主 CTA「下一步」按钮黑色；下载按钮也是黑色（不再是绿色）
- [ ] 国家卡 selected 时，外圈有一圈淡蓝 ring（区分 hover）
- [ ] is-link 国家卡边框为 amber-600，不刺眼
- [ ] Sumbox 整体是中性灰，不再是浅绿喜庆色
- [ ] 必填星号、错误红、清空按钮红色保持
- [ ] 移动端（≤580px）与桌面（≥768px）均无回归
- [ ] `prefers-reduced-motion` 仍生效

## 5. 提交节奏

按现有 fork-only 风格分多次小 commit（与 § 1–12 对应），每次 commit message：

```
style: theme color migration § N → minimal-business [fork-only]
```

最末一次提交附带本 spec 文档与验证清单结果。
