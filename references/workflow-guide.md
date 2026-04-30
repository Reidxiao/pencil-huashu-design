# 标准工作流详细执行指南

> Pencil Design Skill 的完整工作流执行手册。Agent 按照此文档逐步执行，确保每个阶段都不遗漏。

**注意**：本指南基于 SKILL.md 中的 4 阶段流程，详细展开每一步的具体操作。

---

当用户消息中出现以下词汇时，激活 Pencil Design Skill：

| 触发词 | 典型需求 |
|--------|---------|
| 设计、做原型 | 高保真 UI mockup |
| 画界面 | 特定页面/模块 |
| 做个页面 | 单个 HTML 页面 |
| 设计组件 | Button/Card/Form 等组件 |
| UI 设计 | 整体界面设计 |
| 设计稿 | 输出可交付设计 |
| Pencil 设计 | 明确指定 Pencil |

### 需求预处理

1. 提取用户描述的核心需求
2. 判断是否涉及具体品牌（触发品牌资产协议）
3. 判断需求模糊程度（触发 Fallback 的可能性）
4. 判断 Pencil 能力边界（是否适用）

---

## 阶段 1：Design Brief 确认

### 输出物

填写完整的 Design Brief 模板（见 `design-brief-template.md`），并发给用户确认。

### 确认检查点

在发给用户之前，Agent 自己先过一遍：

- [ ] Who 明确了（目标用户）
- [ ] What 明确了（具体要做什么）
- [ ] Why 有了（为什么做，要达成什么）
- [ ] Where 有了（设备/场景）
- [ ] 情绪基调有了（哪怕是"不确定"也比没有好）
- [ ] 假设列出来了（用户需要确认的部分）

### 等待确认

**在此阶段必须等待用户确认后才能进入阶段 2。**

如果用户只说了"做吧"而没有确认 Brief，Agent 应主动询问关键未决问题。

---

## 阶段 2：Existing Context 盘点

### Brand Context（有具体品牌时）

执行品牌资产协议（详见 SKILL.md 主文档）：

```
Step 1: 问（按清单）
Step 2: 搜（官方渠道）
Step 3: 抓（下载 + Pencil 内置资源）
Step 4: 固化（set_variables）
Step 5: 验证（get_screenshot）
```

### Existing Component Context（已有设计系统时）

如果有现有 .pen 文件或设计系统：

1. `batch_get({ action: "get_tree" })` — 了解现有组件结构
2. `batch_get({ action: "list_components" })` — 列出已有组件
3. `get_variables()` — 读取现有变量系统
4. 确定哪些组件可以复用

### Asset Context（图标/图片资源）

- 图标：优先使用 Pencil 内置 icon_font（Lucide / Material / Feather / Phosphor）
- 图片：从 Wikimedia Commons / Met Museum / Unsplash 获取真实图片
- 不要用 emoji 或手绘 SVG

---

## 阶段 3：概念框架

### Frame 骨架设计

在动手之前，先在脑子里画出 Frame 结构：

```
# 假设做一个落地页
canvas
├── header (frame: 1440x80)
├── hero (frame: 1440x600)
│   ├── hero-text-group (frame)
│   └── hero-image (image)
├── features (frame: 1440x400)
│   ├── feature-card-1 (frame)
│   ├── feature-card-2 (frame)
│   └── feature-card-3 (frame)
└── footer (frame: 1440x120)
```

### 组件树设计

识别哪些元素会成为 reusable 组件：

```
# 可复用组件（标记 reusable）
button-primary (frame)
card-article (frame)
input-text (frame)

# 页面专属实例（用 copy）
page-landing-hero
page-landing-features
```

### 变量树设计

在 `set_variables` 之前，先规划变量结构：

```
$color
  ├── brand-primary
  ├── brand-secondary
  ├── ink-primary
  ├── ink-secondary
  ├── surface-default
  └── ...

$spacing
  ├── xs (4)
  ├── sm (8)
  ├── md (16)
  ├── lg (24)
  ├── xl (48)
  └── ...

$font
  ├── display (Newsreader)
  └── body (Inter)
```

---

## 阶段 4：分步创建

### 创建设计元素

**原则：先骨架 → 再组件 → 后实例**

#### Step 4.1：创建主 Frame 骨架

```json
{
  "operations": [
    { "type": "create", "element": { "id": "header", "type": "frame", "width": 1440, "height": 80, "fill": "#FFFFFF" }},
    { "type": "create", "element": { "id": "hero", "type": "frame", "width": 1440, "height": 600, "fill": "#F8F7F4" }},
    { "type": "create", "element": { "id": "features", "type": "frame", "width": 1440, "height": 400, "layout": "horizontal", "gap": 24 }},
    { "type": "create", "element": { "id": "footer", "type": "frame", "width": 1440, "height": 120, "fill": "#1A1A1A" }}
  ]
}
```

#### Step 4.2：截图确认骨架
```
get_screenshot({ frame: "header" })
get_screenshot({ full_page: true })
```

#### Step 4.3：创建 reusable 组件

```json
{
  "operations": [
    { "type": "create", "element": {
      "id": "feature-card",
      "type": "frame",
      "width": 300,
      "height": 200,
      "fill": "#FFFFFF",
      "reusable": true
    }}
  ]
}
```

#### Step 4.4：用 copy 创建实例

```json
{
  "operations": [
    { "type": "copy", "sourceId": "feature-card", "newId": "feature-card-1", "x": 0, "y": 0 },
    { "type": "copy", "sourceId": "feature-card", "newId": "feature-card-2", "x": 324, "y": 0 },
    { "type": "copy", "sourceId": "feature-card", "newId": "feature-card-3", "x": 648, "y": 0 },
    { "type": "insert", "parent": "features", "child": "feature-card-1" },
    { "type": "insert", "parent": "features", "child": "feature-card-2" },
    { "type": "insert", "parent": "features", "child": "feature-card-3" }
  ]
}
```

#### Step 4.5：每步截图确认

每完成一个主要部分，就 `get_screenshot` 确认，发现偏差立即停。

---

## 阶段 5：5 维度自评审

### 执行顺序

1. 读取评审模板（`quality-review-template.md`）
2. 对照模板逐项打分
3. 识别 P0/P1/P2 修复项
4. 计算综合得分

### 自评要点

**哲学一致性：**
- 所有颜色是否来自同一色板？
- 字体组合是否统一（衬线 display + 无衬线 body）？
- 间距是否基于同一网格（8px）？

**视觉层级：**
- 第一眼看到的是最重要的元素吗？
- CTA 够突出吗？

**细节执行：**
- 运行 `snapshot_layout({ action: "check_alignment", grid: 8 })`
- 运行 `snapshot_layout({ action: "find_overlaps" })`

**功能性：**
- 所有交互元素有状态吗（hover/active）？
- 表单有完整状态吗（默认/聚焦/错误）？

**创新性：**
- 有没有让人印象深刻的细节？
- 整体是否有个性？

---

## 阶段 6：修复

### 优先级处理

| 优先级 | 定义 | 处理方式 |
|--------|------|---------|
| P0 | 影响核心功能/可用性 | 必须修复后再交付 |
| P1 | 影响体验/品质感 | 应该修复，如有时间 |
| P2 | 让作品更完美 | 可以修复，属于加分项 |

### 修复后验证

每次修复后：
1. `get_screenshot()` 确认效果
2. 如修复了布局问题，运行 `snapshot_layout` 确认
3. 更新综合得分

---

## 阶段 7：Pre-Delivery Checklist

逐项检查（详见 `pre-delivery-checklist.md`），全部 ✅ 才能进入交付。

---

## 阶段 8：交付

### 交付物清单

| 交付物 | 说明 |
|--------|------|
| .pen 文件 | 源文件 |
| PNG/PDF 截图 | 关键页面/组件存档 |
| 代码导出 | 如需要（React/Vue/HTML） |
| 设计决策记录 | 存入 review log |

### 导出代码

如用户需要代码交付：

```
# 在 Pencil 里选中 Frame 或组件
# 使用 Pencil 内置导出功能，或通过 Code 模式：
"Generate React code for this component"
"Export as HTML with Tailwind CSS"
```

### 版本控制记录

```
# 推荐 commit message 格式
feat(design): add landing page hero section
feat(design): complete landing page with features + footer
fix(design): adjust spacing to 8px grid alignment
refactor(design): extract reusable button component
```

---

## 附录：工作流决策树

```
收到需求
  │
  ├─ 涉及具体品牌？
  │    └─ YES → 品牌资产协议（阶段 2 入口）
  │
  ├─ Pencil 能力边界内？
  │    └─ NO → 立即告知用户，提议替代方案
  │
  ├─ 需求模糊？（情绪基调=不确定）
  │    └─ YES → Fallback 模式 → 用户选方向 → 回到阶段 1
  │
  └─ Design Brief 确认
         └─ 用户未确认 → 等待，不能进阶段 2
```

---

## 常见错误及应对

| 错误 | 后果 | 应对 |
|------|------|------|
| 没确认 Brief 就动手 | 返工率高 | 强制等待确认 |
| 没搜品牌资产就硬做 | 识别度低 | 品牌协议是前置条件 |
| 一次创建太多元素 | 难以定位问题 | 分批创建，每批 screenshot |
| 没用组件复用 | 维护成本高 | 强制先设计 reusable 组件 |
| 忽略 AI slop 检查 | 视觉廉价 | Pre-Delivery Checklist 必过 |
| 没做 5 维度评审 | 品质不稳定 | 强制执行评审 |
