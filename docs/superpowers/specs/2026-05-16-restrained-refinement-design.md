# 精修克制风（Restrained Refinement）设计稿

**日期：** 2026-05-16
**作用对象：** `theme.css`（fork-only 主题层）
**前置：** [[2026-05-15-minimal-business-theme-design]]（在其基础上叠加细节）

## 0. 背景与目标

[[2026-05-15-minimal-business-theme-design]] 完成了主题色彩迁移。本稿在不改色、不改结构的前提下，做 5 组「隐形精修」——目标是让页面看起来"贵一点，但说不出哪里变了"。

灵感参照：Apple HIG、Linear 产品页、Vercel Dashboard、shadcn/ui。这一类视觉的共性是：单看任意一项都不显眼，但十几项叠加在一起，最终给人「精致」感。本稿就是把这十几项明文写出来。

**非目标：**
- 不动色值（spec [[2026-05-15-minimal-business-theme-design]] 已上线）
- 不动 HTML 结构、栅格、断点
- 不引入 Web 字体、不引入 JS、不引入新的 CSS 文件
- 不动 `prefers-reduced-motion` 块（必须仍然让所有 transition 退化）

## 1. 五组精修

### A. 字体渲染与数字精度

```css
/* §1 body 内追加 */
-webkit-font-smoothing: antialiased;
-moz-osx-font-smoothing: grayscale;
text-rendering: optimizeLegibility;
font-feature-settings: "kern" 1, "liga" 1;

/* §1 末新增一条选择器 */
.dot, .sumval, .sumlbl, .sumrow, .fn {
  font-variant-numeric: tabular-nums;
}
```

**为什么：**
- 抗锯齿：mac 系统下字体立刻"清"一档；Windows 不受影响（无副作用）
- text-rendering：高质量 kerning 与连字
- font-feature kern/liga：默认已开但显式声明，确保跨浏览器一致
- tabular-nums：让步骤号 `1/2/3`、汇总数值「合法天数 XX」、字段编号 `.fn` 中的数字纵向对齐——人眼会下意识舒服

**零网络成本，纯 CSS 属性。**

### B. 空间节奏 +4px

```css
.card           { padding: 24px; margin-bottom: 20px; }   /* 旧 20 / 16 */
.ctitle         { padding-bottom: 16px; margin-bottom: 20px; }  /* 旧 12 / 16 */
.grid           { gap: 14px; }   /* 旧 12 */
```

**为什么：** Linear、shadcn/ui、Vercel Dashboard 在工具型应用普遍走 24px 卡片 padding 和 20px 卡间距——比 16/20 多一口气，比 28/32 又不至于松散。在中国大陆中文界面尤其有效（中文字符天生比西文密一档，需要更大的 padding 来"呼吸"）。

**副作用：** 整页高度 +50px 左右（已与用户确认接受）。

### C. 字号字距精修

```css
.hdr h1   { font-size: 22px; letter-spacing: -0.01em; }   /* 旧 20，无字距 */
.ctitle   { font-size: 19px; letter-spacing: -0.005em; }  /* 旧 18，无字距 */
label     { letter-spacing: 0.01em; }
.fn       { letter-spacing: 0.04em; text-transform: uppercase; }
.slabel   { letter-spacing: 0.02em; }

/* §11 (≥768px) 同步抬高 */
.hdr h1 { font-size: 24px; }   /* 旧 22 */
```

**为什么：** 排版的隐形礼仪——大字加紧（标题更挺立，避免松散），小字加宽（标号 / 标签更"标号化"）。

- `.hdr h1`：22px + `-0.01em`，桌面 24px——主标题"主"起来
- `.ctitle`：19px + `-0.005em`——卡片标题与正文 16px 拉开层级
- `label`：14px + `0.01em`——表单标签更精致
- `.fn`：CSS 字段编号（如「PD-1」「FN-2」）大写 + `0.04em`——让标号像标号
- `.slabel`：步骤名称（如「基本信息」）+ `0.02em`——克制的"步骤标题"感

**注意 `.fn uppercase` 仅对英文字母生效**（中文不会大写），所以不会影响中文字段标号。

### D. 过渡曲线统一替换

```css
/* 全文 transition 中所有 "200ms ease" → "200ms cubic-bezier(0.16, 1, 0.3, 1)" */
```

涉及 6 处 transition：
- §3 toolbar 按钮
- §4 .dot
- §6 input/select/textarea
- §7 .ri/.ci
- §8 .btn
- §9 .country-card

**为什么：** 默认 `ease` 是浏览器的"匀加速"，平凡；`cubic-bezier(0.16, 1, 0.3, 1)` 是 Apple、Linear、Stripe 在用的"slow-out"曲线——前 30% 几乎没动，后 70% 急速到位。同样的 200ms duration，但感觉"轻盈"完全不同。

技术上看：曲线参数中 `0.16, 1` 是控制点 1（开始时几乎水平起步），`0.3, 1` 是控制点 2（很快冲到顶端）——形成"先慢后猛"的感觉。

### E. 按下与卡片 lift

```css
.btn:active, .btn-dl:active {
  transform: scale(0.98);
  transition: transform 120ms cubic-bezier(0.16, 1, 0.3, 1);
}

.country-card:hover {
  transform: translateY(-1px);   /* 旧 transform: none */
  box-shadow: 0 2px 6px rgba(15,23,42,.06);   /* 旧 0 1px 3px rgba(15,23,42,.08) */
}
```

**为什么：**
- 按钮按下缩 2%：触觉反馈，无声音的"咔哒"——iOS 的标志性微动效
- 国家卡上浮 1px + 阴影更柔：悬停的"邀请感"——告诉你"我可以被点击"

**为什么 is-link / no-pdf 卡片不上浮：** 它们不属于"主可填写卡片"——`.no-pdf` 是不可交互（占位），`.is-link` 是外部链接（语义不同）。差异化的悬停反馈反而让用户分清"我能在这里填写"和"我点了会跳走"。这是精修——不是"所有卡片上浮"，而是"语义对的卡片才上浮"。

## 2. 实施约束

1. **追加优先于替换。** A 组的字体渲染属性是新加 4 行；B、C、D、E 是修改现有规则。每个修改都最小化 diff。
2. **不引入 Web 字体。** 中文继续用 PingFang SC / Microsoft YaHei，西文继续走 system-ui。原因：网络依赖、首屏 FOUT、加载预算。
3. **不引入 CSS 变量重构。** 沿用 spec [[2026-05-15-minimal-business-theme-design]] § 3.1 的纪律。
4. **`prefers-reduced-motion` 自动覆盖。** §12 块对 `*` 强制 `transition-duration: .01ms !important`，所以新的 cubic-bezier 与 transform 都会自动退化为"无动画"——不需要单独适配。
5. **不动 transform: none 的两类卡片。** §9 中 `.country-card.no-pdf:hover` 与 `.country-card.is-link:hover` 当前都是 `transform: none`，本稿故意保留这种差异。

## 3. 验证清单

实施后人工目视：

- [ ] mac 浏览器中正文文字看起来更"清晰"（抗锯齿生效）
- [ ] 步骤条数字 `1 2 3 4 5 6` 等宽对齐（tabular-nums 生效）
- [ ] 卡片整体感觉"放松"了，padding 明显宽松
- [ ] Header 主标题更醒目（22px → 桌面 24px）
- [ ] CSS 字段编号（如「PD-1」）变成大写小字距
- [ ] hover 任意按钮 / 国家卡时，过渡感觉"先慢后到位"，不是匀速
- [ ] 鼠标按下任意按钮时，按钮明显缩小一点然后弹回
- [ ] 鼠标悬停可填写国家卡时，卡片轻轻上浮 1px
- [ ] 鼠标悬停"链接型"国家卡（amber 边框）时，**不上浮**——保持差异化
- [ ] DevTools 模拟 `prefers-reduced-motion: reduce` 后，所有动效退化为瞬间切换
- [ ] 移动端（≤580px）无回归

## 4. 提交节奏

5 个独立 commit，对应 5 个分组：

```
style: refinement A · font rendering + tabular-nums [fork-only]
style: refinement B · spacing rhythm +4px [fork-only]
style: refinement C · type sizing + letter-spacing [fork-only]
style: refinement D · transition curves → cubic-bezier [fork-only]
style: refinement E · button press + card lift [fork-only]
```
