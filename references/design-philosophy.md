# 20 种设计哲学详解

> 当用户需求模糊时，从这些流派中推荐 3 个差异化方向（必须来自不同流派）。
>
> 前 5 个主流流派附带**可执行 Pencil 参数表**，Agent 可直接用于 `set_variables`。

---

## 流派一：Pentagram 信息建筑派

**代表品牌**：Stripe、Linear、Vercel、Notion、Superhuman

**核心哲学**：信息即建筑，网格即秩序，克制即力量。

**视觉特征**：
- 精密网格系统，8px / 4px 基准网格
- 大量留白，内容密度低
- 单色或双色为主，拒绝多余装饰
- 字体：Grotesk 无衬线（SF Pro、Helvetica Now、Aktiv Grotesk）
- 动效：微妙、精准、有目的

**气质关键词**：`精准` `克制` `网格` `功能至上` `冷静` `专业` `高效`

**适合场景**：
- 开发者工具、SaaS 产品、B2B 软件
- 需要传达"可信赖感"的金融/科技产品
- 目标用户是高认知门槛的专业人士

### 可执行 Pencil 参数

```
set_variables({
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

**设计要点**：颜色不超过 3 种（背景+文字+accent）；间距精确到 8px；不要装饰性图形；字体统一 Inter 或 Aktiv Grotesk。

---

## 流派二：Field.io 运动诗学派

**代表品牌**：Apple、徕卡、Bang & Olufsen、戴森

**核心哲学**：设计是触觉，是光影，是让人想触碰的欲望。

**视觉特征**：
- 光影感：渐变、柔边、发光效果
- 触感：圆角、阴影、玻璃拟态（glassmorphism）
- 空间感：深度层次、模糊背景、景深
- 字体：混合（衬线 display + 无衬线 body）
- 动效：流畅、弹性、物理感

**气质关键词**：`流动` `触感` `光影` `沉浸` `精致` `温暖` `有机` `品质`

**适合场景**：
- 消费品硬件、生活美学产品
- 高端品牌、数字艺术类
- 需要传达"想要拥有"的产品

### 可执行 Pencil 参数

```
set_variables({
  variables: {
    "color-brand-primary": { "type": "color", "value": "#2C2C2E" },
    "color-brand-accent":  { "type": "color", "value": "#C4A484" },
    "color-surface":       { "type": "color", "value": "#FAFAF8" },
    "color-surface-alt":   { "type": "color", "value": "#F5F0EB" },
    "color-ink-primary":   { "type": "color", "value": "#1C1C1E" },
    "color-ink-secondary": { "type": "color", "value": "#8E8E93" },
    "color-border":        { "type": "color", "value": "#E5E5E0" },
    "font-display":        { "type": "string", "value": "Newsreader" },
    "font-body":           { "type": "string", "value": "Inter" },
    "spacing-base":        { "type": "number", "value": 8 },
    "radius-sm":           { "type": "number", "value": 8 },
    "radius-md":           { "type": "number", "value": 16 }
  }
})
```

**设计要点**：使用 gradient fill 和 shadow effects；背景可用细微 radial gradient 模拟光感；暖色调（米白、香槟金、暖灰）；圆角较大（8-16px）；玻璃拟态用半透明白 + 柔和阴影。

---

## 流派三：Kenya Hara 东方极简派

**代表品牌**：无印良品、茑屋书店、故宫博物院

**核心哲学**：减到不能再减，素材本身的美感，做"无品牌的设计"。

**视觉特征**：
- 极度克制：几乎无装饰
- 素材本真：真实摄影、纸质纹理、自然材料视觉
- 排版的艺术：汉字/日文的版式美学
- 字体：思源宋体 / Noto Serif（中文场景），Minion Pro / Caslon（西文）
- 留白：大胆的留白，不是浪费，是呼吸

**气质关键词**：`留白` `本真` `静谧` `克制` `东方` `手工` `诚实` `朴素`

**适合场景**：
- 文化/艺术/出版类产品
- 生活美学品牌
- 需要传达"深度"和"内涵"的产品

### 可执行 Pencil 参数

```
set_variables({
  variables: {
    "color-brand-primary": { "type": "color", "value": "#2D2D2D" },
    "color-brand-accent":  { "type": "color", "value": "#8B7355" },
    "color-surface":       { "type": "color", "value": "#F5F5F0" },
    "color-surface-alt":   { "type": "color", "value": "#FAFAF8" },
    "color-ink-primary":   { "type": "color", "value": "#2D2D2D" },
    "color-ink-secondary": { "type": "color", "value": "#8C8C8C" },
    "color-border":        { "type": "color", "value": "#E0DDD5" },
    "font-display":        { "type": "string", "value": "Noto Serif" },
    "font-body":           { "type": "string", "value": "Noto Sans" },
    "spacing-base":        { "type": "number", "value": 8 },
    "radius-sm":           { "type": "number", "value": 0 },
    "radius-md":           { "type": "number", "value": 2 }
  }
})
```

**设计要点**：背景用接近纸张的暖白（`#F5F5F0`）；衬线字体做标题；不要图标，用文字标签；大量留白，padding 至少 48px 起；圆角极小或为 0。

---

## 流派四：Sagmeister 实验先锋派

**代表品牌**：Spotify、Absolut Vodka、一些独立设计师品牌

**核心哲学**：设计应该让人 WOW，如果不够极端就是在浪费空间。

**视觉特征**：
- 大胆的色彩：撞色、荧光、对比色
- 个性化插图：手绘感、涂鸦感、幽默感
- 打破规则：文字做图形、图形做背景、非常规排版
- 字体：display 字体极端选择（Gestura、Monument Extended）
- 纹理：噪点、划痕、拼贴

**气质关键词**：`大胆` `个性` `反套路` `幽默` `年轻` `活力` `实验` `艺术`

**适合场景**：
- 面向年轻人的消费品牌
- 音乐/娱乐/游戏类产品
- 需要病毒传播的视觉物料

### 可执行 Pencil 参数

```
set_variables({
  variables: {
    "color-brand-primary": { "type": "color", "value": "#FF6B6B" },
    "color-brand-accent":  { "type": "color", "value": "#4ECDC4" },
    "color-surface":       { "type": "color", "value": "#FFFDF7" },
    "color-surface-alt":   { "type": "color", "value": "#2C2C2E" },
    "color-ink-primary":   { "type": "color", "value": "#1A1A1A" },
    "color-ink-secondary": { "type": "color", "value": "#FF6B6B" },
    "color-border":        { "type": "color", "value": "#FF6B6B" },
    "font-display":        { "type": "string", "value": "Monument Extended" },
    "font-body":           { "type": "string", "value": "Space Grotesk" },
    "spacing-base":        { "type": "number", "value": 8 },
    "radius-sm":           { "type": "number", "value": 0 },
    "radius-md":           { "type": "number", "value": 4 }
  }
})
```

**设计要点**：颜色可以用高饱和 accent（`#FF6B6B`、`#4ECDC4`）；字体选有个性的 display；图形可以手绘风格或几何抽象；可以用 Pencil 的 icon_font 选有个性的 icon；可以故意打破网格。

---

## 流派五：北欧功能主义派

**代表品牌**：IKEA、HAY、Beoplay

**核心哲学**：形式追随功能，但形式本身也要美。好的设计是看不到设计痕迹的。

**视觉特征**：
- 功能美学：每个元素都有存在的理由
- 人性化比例：不是冰冷的比例，是人用的比例
- 自然材料视觉：木头、皮革、羊毛的视觉语言
- 温暖色调：暖灰、木色、脏粉色、芥末黄
- 字体：几何无衬线（Futura Now、Akkurat、Proxima Nova）

**气质关键词**：`实用` `温暖` `人性化` `北欧` `功能` `简约` `自然` `民主`

**适合场景**：
- 家居/生活方式品牌
- 教育类产品
- 需要传达"亲切感"而非"权威感"的产品

### 可执行 Pencil 参数

```
set_variables({
  variables: {
    "color-brand-primary": { "type": "color", "value": "#5C5C5C" },
    "color-brand-accent":  { "type": "color", "value": "#D4AF37" },
    "color-surface":       { "type": "color", "value": "#F8F5F0" },
    "color-surface-alt":   { "type": "color", "value": "#E8DED1" },
    "color-ink-primary":   { "type": "color", "value": "#2D2D2D" },
    "color-ink-secondary": { "type": "color", "value": "#8C8C8C" },
    "color-border":        { "type": "color", "value": "#D4CFC7" },
    "font-display":        { "type": "string", "value": "Proxima Nova" },
    "font-body":           { "type": "string", "value": "Proxima Nova" },
    "spacing-base":        { "type": "number", "value": 8 },
    "radius-sm":           { "type": "number", "value": 8 },
    "radius-md":           { "type": "number", "value": 12 }
  }
})
```

**设计要点**：暖色调（`#E8DED1`、`#C4B7A6`）；几何无衬线字体；图标用线性风格；间距略微宽松体现呼吸感；圆角中等（8-12px）。

---

## 其他 15 种补充方向（可混搭）

### 6. 瑞士国际主义（International Typographic Style）
**代表**：IBM、Air France
**关键词**：`网格` `无衬线` `Helvetica` `红蓝` `清晰`

### 7. 包豪斯现代主义
**代表**：DIOR LOGO 早期、Vitra
**关键词**：`几何` `原色` `基础形状` `功能`

### 8. 孟菲斯设计（Memphis）
**代表**：Daniel Wellington、独立设计师品牌
**关键词**：`几何图案` `撞色` `层叠` `趣味` `反主流`

### 9. 新丑风（New Ugly）
**代表**：独立杂志、TikTok 视觉
**关键词**：`故意丑` `高对比` `混乱` `真实` `反精致`

### 10. 赛博朋克/数字美学
**代表**：Cyberpunk 2077、Razer
**关键词**：`霓虹` `暗色` `故障艺术` `网格`

### 11. 极简主义（Minimal）
**代表**：Balenciaga、Rick Owens
**关键词**：`黑与白` `极少元素` `排他性` `张力`

### 12. 复古/怀旧（Nostalgia）
**代表**：消费品牌、汽水包装
**关键词**：`80-90 年代` `霓虹字体` `暖色调` `质感`

### 13. 植物系/自然系
**代表**：Aesop
**关键词**：`大地色` `植物` `天然` `手工感`

### 14. 漫画/波普艺术
**代表**：KAWS、街头品牌
**关键词**：`高饱和` `网点` `粗黑线` `名人形象`

### 15. 玻璃拟态（Glassmorphism）
**代表**：iOS 设计、macOS 界面
**关键词**：`磨砂玻璃` `半透明` `背景模糊` `柔边`

### 16. 瑞士朋克（Swiss Punk）
**代表**：Massimo Vignelli 晚期
**关键词**：`故意打破网格` `冲突字体` `激进`

### 17. 扁平化 2.0（Flat 2.0）
**代表**：Slack、Spotify
**关键词**：`微渐变` `长阴影` `简化但有深度`

### 18. 像素/8-bit 复古
**代表**：独立游戏、怀旧产品
**关键词**：`像素` `网格` `限制色板` `低分辨率`

### 19. 酸性设计（Acid Design）
**代表**：地下音乐节视觉
**关键词**：`荧光` `扭曲` `高对比` `迷幻`

### 20. 人文主义设计（Humanist）
**代表**：Google 早期、GitHub
**关键词**：`圆润` `温暖` `可亲近` `无衬线`

---

## 混搭技巧

不同流派可以混搭，但要注意**主次关系**：

| 场景 | 主风格 | 辅风格 | 效果 |
|------|--------|--------|------|
| 科技产品但要有人情味 | Pentagram 派（60%） | 北欧功能（40%） | 专业但不冷漠 |
| 高端品牌但要年轻 | 东方极简（50%） | 实验先锋（50%） | 有深度但不无聊 |
| 金融产品但要不无聊 | 瑞士国际（70%） | 孟菲斯（30%） | 权威但有活力 |

---

## 选择流程

当用户需求模糊时：

1. **问关键问题**：
   - "高端感还是接地气？"
   - "保守稳健还是大胆创新？"
   - "科技感还是人文感？"

2. **缩小到 2-3 个流派**

3. **用对应流派的 `set_variables` 参数直接创建设计**

4. **get_screenshot 展示结果**

---

## 版本历史

| 版本 | 日期 | 变更内容 |
|------|------|---------|
| v1.0.0 | 2026-04-30 | 初始版本 |
