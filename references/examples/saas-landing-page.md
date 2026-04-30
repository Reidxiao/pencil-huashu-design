# 端到端示例：SaaS 落地页设计

> 完整展示从用户 prompt 到交付的全过程，包含每一步的 MCP 调用。

---

## 用户 Prompt

```
帮我设计一个 AI 写作助手的落地页，叫"WriteFlow"。目标用户是内容创作者和营销团队。
要简洁专业，不要太花哨。
```

---

## 阶段 A：Design Brief

### Agent 填写的 Brief

```
## Design Brief

### 5W1H
- Who：内容创作者、营销团队（25-40 岁，熟悉数字工具）
- What：WriteFlow AI 写作助手的营销落地页
- Why：展示产品价值，引导用户注册试用
- Where：桌面端 Web（1440px 宽度）
- How：.pen 文件，可导出 PNG/HTML

### 情绪基调
冷淡专业（Pentagram 信息建筑派）

### 品牌约束
- Logo：❌ 无（用文字 Logo）
- 品牌色：待定（建议深色主调 + 蓝色 accent）
- 参考竞品：Notion、Linear、Jasper

### 功能需求
1. Hero 区：产品名 + 一句话价值主张 + CTA 按钮
2. Features 区：3 个核心功能卡片
3. Social Proof 区：用户评价或数据
4. CTA 区：底部注册引导
```

### Agent 向用户确认

> Brief 我整理好了。几个关键点确认：
> 1. 情绪基调选"冷淡专业"（类似 Linear/Notion 风格），可以吗？
> 2. 品牌主色我建议用深灰 `#1A1A1A` + 蓝色 accent `#635BFF`，你觉得呢？
> 3. 桌面端 1440px 宽度，不做移动端？

### 用户回复

> 可以，就按这个来。

---

## 阶段 B：设计规划

### 1. 能力边界判断

- 复杂 3D / 物理模拟 → ❌ 不需要
- 动态数据实时更新 → ❌ 不需要（静态页面）
- 拖拽交互 → ❌ 不需要
- ✅ Pencil 适用

### 2. 盘点已有组件

```
batch_get({
  filePath: "writeflow.pen",
  patterns: [{ reusable: true }],
  readDepth: 2
})
```

返回：空（新文件，无已有组件）

### 3. 设计方向

用户已确认"Pentagram 信息建筑派"，不需要 Fallback。

### 4. Frame 骨架

```
canvas
├── header (1440x72) — 导航栏
├── hero (1440x560) — Hero 区
├── features (1440x480) — 功能介绍
├── proof (1440x320) — 社会证明
├── cta-bottom (1440x240) — 底部 CTA
└── footer (1440x80) — 页脚
```

### 5. 组件树

```
Reusable 组件：
- button-primary (frame, reusable)
- card-feature (frame, reusable)
- testimonial-card (frame, reusable)

页面专属：
- header-nav
- hero-content
- features-grid
```

### 6. 变量设计

采用 Pentagram 派参数（见 design-philosophy.md 流派一）。

---

## 阶段 C：构建

### Step 1：设置变量

```
set_variables({
  filePath: "writeflow.pen",
  variables: {
    "color-brand-primary": { "type": "color", "value": "#1A1A1A" },
    "color-brand-accent":  { "type": "color", "value": "#635BFF" },
    "color-surface":       { "type": "color", "value": "#FFFFFF" },
    "color-surface-alt":   { "type": "color", "value": "#F6F9FC" },
    "color-ink-primary":   { "type": "color", "value": "#1A1A1A" },
    "color-ink-secondary": { "type": "color", "value": "#697386" },
    "color-border":        { "type": "color", "value": "#E3E8EE" },
    "font-display":        { "type": "string", "value": "Inter" },
    "font-body":           { "type": "string", "value": "Inter" },
    "spacing-base":        { "type": "number", "value": 8 },
    "radius-sm":           { "type": "number", "value": 4 },
    "radius-md":           { "type": "number", "value": 6 }
  }
})
```

### Step 2：创建主 Frame 骨架

```
batch_design({
  filePath: "writeflow.pen",
  operations: `
    header = I(document, { type: "frame", name: "header", w: 1440, h: 72, fill: "$color-surface", layout: "horizontal", padding: [0, 64], justifyContent: "space_between", alignItems: "center" })
    hero = I(document, { type: "frame", name: "hero", w: 1440, h: 560, fill: "$color-surface", layout: "vertical", padding: [120, 64], gap: 32 })
    features = I(document, { type: "frame", name: "features", w: 1440, h: 480, fill: "$color-surface-alt", layout: "vertical", padding: [80, 64], gap: 48 })
    proof = I(document, { type: "frame", name: "proof", w: 1440, h: 320, fill: "$color-surface", layout: "vertical", padding: [80, 64], gap: 32 })
    ctaBottom = I(document, { type: "frame", name: "cta-bottom", w: 1440, h: 240, fill: "$color-brand-primary", layout: "vertical", padding: [64, 64], gap: 24, alignItems: "center", justifyContent: "center" })
    footer = I(document, { type: "frame", name: "footer", w: 1440, h: 80, fill: "$color-surface-alt", layout: "horizontal", padding: [0, 64], alignItems: "center" })
  `
})
```

### Step 3：截图确认骨架

```
get_screenshot({ filePath: "writeflow.pen", nodeId: "hero" })
```

→ 确认布局方向正确，继续。

### Step 4：创建 reusable 组件

```
batch_design({
  filePath: "writeflow.pen",
  operations: `
    btnComp = I(document, { type: "frame", name: "button-primary", w: 160, h: 48, fill: "$color-brand-accent", reusable: true, cornerRadius: "$radius-sm", layout: "horizontal", justifyContent: "center", alignItems: "center", gap: 8 })
    I(btnComp, { type: "text", name: "btn-label", content: "Get Started", fontSize: 15, fontFamily: "$font-body", fontWeight: 600, fill: "#FFFFFF" })

    cardComp = I(document, { type: "frame", name: "card-feature", w: 380, h: 280, fill: "$color-surface", reusable: true, cornerRadius: "$radius-md", layout: "vertical", padding: 32, gap: 16 })
    I(cardComp, { type: "icon_font", name: "card-icon", iconFontFamily: "lucide", icon: "zap", fontSize: 32, fill: "$color-brand-accent" })
    I(cardComp, { type: "text", name: "card-title", content: "Feature Title", fontSize: 20, fontFamily: "$font-display", fontWeight: 700, fill: "$color-ink-primary" })
    I(cardComp, { type: "text", name: "card-desc", content: "Feature description goes here with enough detail.", fontSize: 15, fontFamily: "$font-body", fontWeight: 400, fill: "$color-ink-secondary", lineHeight: 1.6 })
  `
})
```

### Step 5：填充 Header

```
batch_design({
  filePath: "writeflow.pen",
  operations: `
    logo = I(header, { type: "text", name: "logo-text", content: "WriteFlow", fontSize: 20, fontFamily: "$font-display", fontWeight: 700, fill: "$color-ink-primary" })
    navGroup = I(header, { type: "frame", name: "nav-links", layout: "horizontal", gap: 32 })
    I(navGroup, { type: "text", name: "nav-features", content: "Features", fontSize: 14, fontFamily: "$font-body", fill: "$color-ink-secondary" })
    I(navGroup, { type: "text", name: "nav-pricing", content: "Pricing", fontSize: 14, fontFamily: "$font-body", fill: "$color-ink-secondary" })
    I(navGroup, { type: "text", name: "nav-docs", content: "Docs", fontSize: 14, fontFamily: "$font-body", fill: "$color-ink-secondary" })
    headerBtn = C("button-primary", { newId: "header-cta", x: 0, y: 0 })
    M("header-cta", "header", 2)
    U("header-cta", { descendants: { "btn-label": { content: "Sign Up" } }, w: 120, h: 40 })
  `
})
```

### Step 6：填充 Hero

```
batch_design({
  filePath: "writeflow.pen",
  operations: `
    I(hero, { type: "text", name: "hero-eyebrow", content: "AI-POWERED WRITING", fontSize: 12, fontFamily: "$font-body", fontWeight: 600, fill: "$color-brand-accent", letterSpacing: 2 })
    I(hero, { type: "text", name: "hero-title", content: "Write better content,\nfaster than ever.", fontSize: 56, fontFamily: "$font-display", fontWeight: 700, fill: "$color-ink-primary", lineHeight: 1.1 })
    I(hero, { type: "text", name: "hero-subtitle", content: "WriteFlow helps marketing teams create high-quality blog posts, ads, and emails in minutes — not hours.", fontSize: 18, fontFamily: "$font-body", fontWeight: 400, fill: "$color-ink-secondary", lineHeight: 1.6 })
    heroBtnGroup = I(hero, { type: "frame", name: "hero-cta-group", layout: "horizontal", gap: 16 })
    heroCta = C("button-primary", { newId: "hero-cta", x: 0, y: 0 })
    M("hero-cta", "hero-cta-group", 0)
    U("hero-cta", { descendants: { "btn-label": { content: "Start Free Trial" } } })
    I(heroBtnGroup, { type: "text", name: "hero-cta-secondary", content: "No credit card required", fontSize: 14, fontFamily: "$font-body", fill: "$color-ink-secondary" })
  `
})
```

### Step 7：填充 Features

```
batch_design({
  filePath: "writeflow.pen",
  operations: `
    I(features, { type: "text", name: "features-heading", content: "Everything you need to write at scale.", fontSize: 36, fontFamily: "$font-display", fontWeight: 700, fill: "$color-ink-primary" })
    featuresGrid = I(features, { type: "frame", name: "features-grid", layout: "horizontal", gap: 24 })
    card1 = C("card-feature", { newId: "feature-card-1", x: 0, y: 0 })
    card2 = C("card-feature", { newId: "feature-card-2", x: 404, y: 0 })
    card3 = C("card-feature", { newId: "feature-card-3", x: 808, y: 0 })
    M("feature-card-1", "features-grid", 0)
    M("feature-card-2", "features-grid", 1)
    M("feature-card-3", "features-grid", 2)
    U("feature-card-1", { descendants: { "card-icon": { icon: "sparkles" }, "card-title": { content: "Smart Generation" }, "card-desc": { content: "Generate blog posts, ad copy, and emails from a simple brief. AI understands your brand voice." } } })
    U("feature-card-2", { descendants: { "card-icon": { icon: "users" }, "card-title": { content: "Team Collaboration" }, "card-desc": { content: "Real-time editing, comments, and approval workflows. Keep your entire team aligned." } } })
    U("feature-card-3", { descendants: { "card-icon": { icon: "bar-chart-3" }, "card-title": { content: "Content Analytics" }, "card-desc": { content: "Track performance across channels. Know what resonates and optimize your strategy." } } })
  `
})
```

### Step 8：填充 Proof + CTA + Footer

```
batch_design({
  filePath: "writeflow.pen",
  operations: `
    I(proof, { type: "text", name: "proof-stat-number", content: "10,000+", fontSize: 48, fontFamily: "$font-display", fontWeight: 700, fill: "$color-brand-accent" })
    I(proof, { type: "text", name: "proof-stat-label", content: "content creators trust WriteFlow", fontSize: 18, fontFamily: "$font-body", fill: "$color-ink-secondary" })

    I(ctaBottom, { type: "text", name: "cta-heading", content: "Ready to write faster?", fontSize: 36, fontFamily: "$font-display", fontWeight: 700, fill: "#FFFFFF" })
    I(ctaBottom, { type: "text", name: "cta-sub", content: "Start your 14-day free trial. No credit card required.", fontSize: 16, fontFamily: "$font-body", fill: "#FFFFFF", opacity: 0.8 })
    ctaBtn = C("button-primary", { newId: "bottom-cta", x: 0, y: 0 })
    M("bottom-cta", "cta-bottom", 3)
    U("bottom-cta", { fill: "#FFFFFF", descendants: { "btn-label": { content: "Get Started Free", fill: "$color-brand-primary" } } })

    I(footer, { type: "text", name: "footer-copy", content: "© 2026 WriteFlow. All rights reserved.", fontSize: 13, fontFamily: "$font-body", fill: "$color-ink-secondary" })
  `
})
```

### Step 9：全局截图确认

```
get_screenshot({ filePath: "writeflow.pen", nodeId: "header" })
get_screenshot({ filePath: "writeflow.pen", nodeId: "hero" })
get_screenshot({ filePath: "writeflow.pen", nodeId: "features" })
```

---

## 阶段 D：评审

### 5 维度自评

| 维度 | 得分 | 说明 |
|------|------|------|
| 哲学一致性 | 8/10 | 统一的 Pentagram 风格，颜色/字体/间距一致 |
| 视觉层级 | 8/10 | Hero 标题最大，CTA 突出，层级清晰 |
| 细节执行 | 7/10 | 需检查对齐和间距一致性 |
| 功能性 | 7/10 | 缺少 hover 状态（静态稿限制） |
| 创新性 | 6/10 | 标准 SaaS 落地页，无特别亮点 |
| **综合** | **7.3/10** | |

### layout 检查

```
snapshot_layout({ filePath: "writeflow.pen", problemsOnly: true })
```

→ 无重叠/裁剪问题。

### 修复

- P1：features 区标题和卡片之间的间距偏小 → 调整 gap
- P2：proof 区数字可以更大更有冲击力

### Pre-Delivery Checklist

- [x] 所有颜色用变量
- [x] 无硬性禁止 slop 项
- [x] 蓝色 accent 有理由（Pentagram 派 + SaaS 场景）
- [x] 无布局重叠
- [x] 组件 ID 语义化
- [x] reusable 组件正确标记

---

## 交付

```
export_nodes({
  filePath: "writeflow.pen",
  nodeIds: ["hero", "features"],
  outputDir: "D:/exports",
  format: "png",
  scale: 2
})
```

---

## 关键 Takeaway

1. **分批创建**：先骨架 → 截图确认 → 再填充内容 → 再截图确认
2. **变量先行**：`set_variables` 在 `batch_design` 之前
3. **组件复用**：button-primary 被 3 处复用，card-feature 被 3 处复用
4. **descendants 定制**：每个实例的内容不同，但结构一致
5. **每步截图**：不要一口气做完再看，发现偏差立即停
