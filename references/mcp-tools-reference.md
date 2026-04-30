# MCP 工具完整参数参考 · Pencil 版

> 基于 Pencil MCP 实际 API 整理。**所有示例均为实际调用格式**，可直接使用。

---

## 工具速查表

| 工具 | 用途 | 常用场景 |
|------|------|---------|
| `batch_design` | 创建/修改/复制/移动/删除元素 | 核心设计操作 |
| `batch_get` | 读取设计结构和内容 | 了解画布状态 |
| `get_screenshot` | 渲染设计预览 | 验证视觉效果 |
| `snapshot_layout` | 分析布局问题 | 检查对齐/重叠 |
| `get_variables` | 读取设计变量 | 查看当前 token |
| `set_variables` | 设置/更新变量 | 定义品牌 token |
| `get_editor_state` | 获取编辑器状态 | 确认当前文件 |
| `find_empty_space_on_canvas` | 查找空白区域 | 定位新元素放置位置 |
| `open_document` | 打开/新建 .pen 文件 | 开始新设计 |
| `export_nodes` | 导出节点为图片/PDF | 交付物生成 |

---

## 前置操作：确认连接

每次设计开始前，先确认 Pencil MCP 已连接：

```json
// get_editor_state — 获取当前编辑器状态
{ "include_schema": true }
```

返回值包含 `active_file`（当前打开的 .pen 文件路径）、`selection`（当前选中元素）等。

如果需要打开或新建文件：

```json
// 打开已有文件
{ "filePathOrTemplate": "D:/path/to/design.pen" }

// 新建文件
{ "filePathOrTemplate": "new" }
```

---

## batch_design

**用途**：创建、修改、复制、移动、删除设计元素

### 参数

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| `filePath` | string | 是 | .pen 文件路径 |
| `operations` | string | 是 | JavaScript 语法的操作列表 |

### operations 语法

operations 使用 JavaScript 函数调用语法，支持以下操作函数：

| 函数 | 用途 | 说明 |
|------|------|------|
| `I(parent, data)` | Insert — 创建并插入子元素 | 必须指定 parent，返回新节点的 ID binding |
| `U(path, changes)` | Update — 修改节点属性 | path 可以是节点 ID 或 binding + "/childId" |
| `C(source, options)` | Copy — 复制节点 | source 是源节点 ID，options 可指定 newId/x/y |
| `M(nodeId, target, index)` | Move — 移动节点到新父级 | |
| `D(nodeId)` | Delete — 删除节点 | |
| `R(nodeId, newData)` | Replace — 替换节点 | |

**重要规则**：
- 每行一个操作调用，可带 binding 赋值
- 操作按顺序执行
- binding 名称在整个 operations 列表中必须唯一
- document 是预定义 binding，代表文档根节点

### 创建基本元素

```
// 创建 Frame 容器
header = I(document, {
  type: "frame",
  name: "header",
  x: 0, y: 0,
  w: 1440, h: 80,
  fill: "$color-surface",
  layout: "horizontal",
  gap: 24,
  padding: 16
})

// 创建文本
title = I(header, {
  type: "text",
  content: "Welcome",
  fontSize: 32,
  fontFamily: "$font-display",
  fontWeight: 700,
  fill: "$color-ink-primary"
})

// 创建矩形
divider = I(header, {
  type: "rectangle",
  w: 1, h: 40,
  fill: "$color-border"
})

// 创建图标（必须作为 frame 的子元素）
iconBtn = I(header, { type: "frame", w: 40, h: 40, fill: "transparent" })
I(iconBtn, {
  type: "icon_font",
  iconFontFamily: "lucide",
  icon: "menu",
  fontSize: 24,
  fill: "$color-ink-primary"
})
```

### Frame layout 属性

| 属性 | 值 | 说明 |
|------|----|------|
| `layout` | `"vertical"` / `"horizontal"` / `"none"` | flex 布局方向。**Frame 默认是 horizontal**，Group 默认是 none |
| `gap` | number | 子元素间距 |
| `padding` | number / [h, v] / [t, r, b, l] | 内边距 |
| `justifyContent` | `"start"` / `"center"` / `"end"` / `"space_between"` / `"space_around"` | 主轴对齐 |
| `alignItems` | `"start"` / `"center"` / `"end"` / `"stretch"` | 交叉轴对齐 |

### text 元素属性

| 属性 | 值 | 说明 |
|------|----|------|
| `content` | string | 文本内容 |
| `fontSize` | number | 字号（px） |
| `fontFamily` | string | 字体名（如 `"Newsreader"`、`"Inter"`），可用 `"$font-display"` 引用变量 |
| `fontWeight` | number | 字重（400/500/700） |
| `lineHeight` | number | 行高（建议 1.2-1.7） |
| `textAlign` | `"left"` / `"center"` / `"right"` | 文本对齐 |
| `fill` | string | 文本颜色，支持 `"$variable"` 引用 |

### 创建 Reusable 组件（带 Slot）

```
// 创建可复用卡片组件
cardComp = I(document, {
  type: "frame",
  name: "card-component",
  w: 320, h: 200,
  fill: "$color-surface",
  reusable: true,
  cornerRadius: 8
})

// 组件内部结构
cardHeader = I(cardComp, {
  type: "frame",
  name: "card-header",
  layout: "horizontal",
  padding: 16,
  h: 48
})

cardSlot = I(cardComp, {
  type: "frame",
  name: "card-content-slot",
  slot: ["text", "image"],
  fill: "$color-surface-alt"
})

cardFooter = I(cardComp, {
  type: "frame",
  name: "card-footer",
  layout: "horizontal",
  padding: 16,
  h: 48
})
```

### 复制组件创建实例

```
// 用 C() 创建实例
card1 = C("card-component", { newId: "article-card-1", x: 0, y: 200 })
card2 = C("card-component", { newId: "article-card-2", x: 344, y: 200 })
card3 = C("card-component", { newId: "article-card-3", x: 688, y: 200 })

// 将实例插入父容器
M("article-card-1", "features-section", 0)
M("article-card-2", "features-section", 1)
M("article-card-3", "features-section", 2)
```

### 用 descendants 定制实例

```
// 定制实例的子元素（不破坏与原点的链接）
U("article-card-1", {
  descendants: {
    "card-header": { fill: "$color-brand-primary" },
    "card-content-slot": {
      children: [
        { id: "slot-text-1", type: "text", content: "Article title here", fontSize: 16 }
      ]
    }
  }
})
```

**关键规则**：
- 实例用 `C()`（copy）创建，**不要**自己构造 `type: "ref"`
- 定制实例用 `descendants`，**不要**直接改实例根属性（会断开链接）
- `icon_font` 必须作为 `frame` 的子元素，不能是顶层元素
- `slot` 值必须是数组（如 `["text", "image"]`，不是 `"text"`）

### 修改属性

```
// 修改已有元素的属性
U("hero-title", { fontSize: 96, fill: "$color-brand-accent" })

// 修改布局
U("features-section", { gap: 32, padding: [48, 64] })
```

### 删除和移动

```
// 删除元素
D("unwanted-element")

// 移动元素到新父级
M("sidebar-widget", "main-content", 0)
```

### 替换元素

```
// 替换一个元素为新元素
R("old-placeholder", {
  type: "frame",
  name: "new-component",
  w: 200, h: 100,
  fill: "$color-surface"
})
```

---

## batch_get

**用途**：读取设计结构和内容

### 参数

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| `filePath` | string | 是 | .pen 文件路径 |
| `nodeIds` | string[] | 否 | 指定要读取的节点 ID 列表 |
| `patterns` | array | 否 | 搜索模式列表 |
| `parentId` | string | 否 | 限定搜索范围的父节点 ID |
| `readDepth` | number | 否 | 读取深度（默认 1，仅返回直接子节点） |
| `searchDepth` | number | 否 | 搜索深度（默认无限制） |
| `resolveInstances` | boolean | 否 | true 时展开组件实例为完整结构 |
| `resolveVariables` | boolean | 否 | true 时显示变量的实际计算值而非引用 |
| `includePathGeometry` | boolean | 否 | true 时返回 path 节点的完整几何数据 |

### 常用调用模式

**读取文档顶层结构**（不传任何参数）：

```json
{ "filePath": "design.pen" }
```

返回文档的顶层子节点列表。

**读取特定节点的子节点**：

```json
{
  "filePath": "design.pen",
  "nodeIds": ["header", "hero-section"],
  "readDepth": 2
}
```

**搜索 reusable 组件**：

```json
{
  "filePath": "design.pen",
  "patterns": [{ "reusable": true }],
  "readDepth": 2,
  "searchDepth": 3
}
```

**按类型搜索**：

```json
{
  "filePath": "design.pen",
  "patterns": [{ "type": "text" }],
  "parentId": "hero-section"
}
```

**patterns 支持的字段**：

| 字段 | 类型 | 说明 |
|------|------|------|
| `name` | string | 节点名称（正则匹配） |
| `reusable` | boolean | 是否为 reusable 组件 |
| `type` | enum | 元素类型：`frame` / `group` / `rectangle` / `ellipse` / `line` / `polygon` / `path` / `text` / `connection` / `note` / `icon_font` / `image` / `ref` |

**展开组件实例查看完整结构**：

```json
{
  "filePath": "design.pen",
  "nodeIds": ["article-card-1"],
  "resolveInstances": true,
  "readDepth": 3
}
```

---

## get_screenshot

**用途**：渲染设计预览

### 参数

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| `filePath` | string | 是 | .pen 文件路径 |
| `nodeId` | string | 是 | 要渲染的节点 ID |

### 调用示例

```json
// 渲染整个页面/主 Frame
{ "filePath": "design.pen", "nodeId": "page-home" }

// 渲染单个组件
{ "filePath": "design.pen", "nodeId": "hero-section" }
```

**注意**：`get_screenshot` 需要指定具体的 `nodeId`。要渲染整个画布，使用文档根节点或主 Frame 的 ID。

---

## snapshot_layout

**用途**：分析布局问题（重叠、对齐等）

### 参数

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| `filePath` | string | 是 | .pen 文件路径 |
| `maxDepth` | number | 否 | 分析深度（0 = 仅顶层，不传 = 无限制） |
| `parentId` | string | 否 | 限定分析范围的父节点 |
| `problemsOnly` | boolean | 否 | true = 仅返回有问题的节点（重叠、裁剪等） |

### 调用示例

**查看顶层布局**（找新 Frame 放哪）：

```json
{ "filePath": "design.pen", "maxDepth": 0 }
```

**分析特定区域的布局问题**：

```json
{
  "filePath": "design.pen",
  "parentId": "main-content",
  "problemsOnly": true
}
```

**完整布局分析**：

```json
{ "filePath": "design.pen" }
```

---

## get_variables

**用途**：读取当前文档的所有变量和主题

### 参数

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| `filePath` | string | 是 | .pen 文件路径 |

### 调用

```json
{ "filePath": "design.pen" }
```

---

## set_variables

**用途**：设置/更新变量

### 参数

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| `filePath` | string | 是 | .pen 文件路径 |
| `variables` | object | 是 | 变量定义对象 |
| `replace` | boolean | 否 | true = 完全替换现有变量（默认 false = 合并） |

### 变量值格式

每个变量必须是 `{ type, value }` 对象，type 可以是 `"color"` / `"number"` / `"string"` / `"boolean"`。

**简单变量**：

```json
{
  "filePath": "design.pen",
  "variables": {
    "color-brand-primary": { "type": "color", "value": "#1783FF" },
    "color-surface": { "type": "color", "value": "#FFFFFF" },
    "spacing-base": { "type": "number", "value": 8 },
    "font-display": { "type": "string", "value": "Newsreader" },
    "font-body": { "type": "string", "value": "Inter" }
  }
}
```

**主题感知变量**（支持 light/dark 模式）：

```json
{
  "filePath": "design.pen",
  "variables": {
    "color-brand-primary": {
      "type": "color",
      "value": [
        { "value": "#1783FF", "theme": { "mode": "light" } },
        { "value": "#60A5FA", "theme": { "mode": "dark" } }
      ]
    },
    "color-surface": {
      "type": "color",
      "value": [
        { "value": "#FFFFFF", "theme": { "mode": "light" } },
        { "value": "#1A1A1A", "theme": { "mode": "dark" } }
      ]
    }
  }
}
```

**变量引用语法**：在元素属性中用 `"$variable-name"` 引用变量，如 `fill: "$color-brand-primary"`。

---

## find_empty_space_on_canvas

**用途**：在画布上查找空白区域，用于定位新元素的放置位置

### 参数

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| `filePath` | string | 是 | .pen 文件路径 |
| `width` | number | 是 | 所需区域宽度 |
| `height` | number | 是 | 所需区域高度 |
| `padding` | number | 是 | 与其他元素的最小间距 |
| `direction` | enum | 是 | 搜索方向：`"top"` / `"right"` / `"bottom"` / `"left"` |
| `nodeId` | string | 否 | 从指定节点周围查找（不传则从整个画布内容查找） |

### 调用示例

```json
// 在画布右侧找一块 1440x900 的空间
{
  "filePath": "design.pen",
  "width": 1440,
  "height": 900,
  "padding": 100,
  "direction": "right"
}

// 在某个 Frame 下方找空间
{
  "filePath": "design.pen",
  "nodeId": "page-home",
  "width": 1440,
  "height": 600,
  "padding": 50,
  "direction": "bottom"
}
```

---

## export_nodes

**用途**：导出节点为图片或 PDF

### 参数

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| `filePath` | string | 是 | .pen 文件路径 |
| `nodeIds` | string[] | 是 | 要导出的节点 ID 列表 |
| `outputDir` | string | 是 | 输出目录 |
| `format` | enum | 否 | `"png"` / `"jpeg"` / `"webp"` / `"pdf"`（默认 png） |
| `scale` | number | 否 | 缩放比例（默认 2） |
| `quality` | number | 否 | JPEG/WEBP 质量 1-100 |

### 调用示例

```json
// 导出为 PNG
{
  "filePath": "design.pen",
  "nodeIds": ["page-home", "page-about"],
  "outputDir": "D:/exports",
  "format": "png",
  "scale": 2
}

// 导出为 PDF（所有节点合为一个文档）
{
  "filePath": "design.pen",
  "nodeIds": ["page-home", "page-about"],
  "outputDir": "D:/exports",
  "format": "pdf"
}
```

---

## search_all_unique_properties

**用途**：递归搜索节点树中的所有唯一属性值，用于了解当前设计中使用了哪些颜色、字体等

### 参数

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| `filePath` | string | 是 | .pen 文件路径 |
| `parents` | string[] | 是 | 搜索的父节点 ID 列表 |
| `properties` | string[] | 是 | 要搜索的属性列表 |

### properties 支持的值

`fillColor` / `textColor` / `strokeColor` / `strokeThickness` / `cornerRadius` / `padding` / `gap` / `fontSize` / `fontFamily` / `fontWeight`

### 调用示例

```json
// 查看设计中使用了哪些颜色和字体
{
  "filePath": "design.pen",
  "parents": ["page-home"],
  "properties": ["fillColor", "textColor", "fontFamily", "fontSize"]
}
```

---

## replace_all_matching_properties

**用途**：批量替换节点树中的属性值（如统一更换颜色、字体等）

### 参数

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| `filePath` | string | 是 | .pen 文件路径 |
| `parents` | string[] | 是 | 操作的父节点 ID 列表 |
| `properties` | object | 是 | 替换规则 |

### 调用示例

```json
// 将所有旧品牌色替换为新品牌色
{
  "filePath": "design.pen",
  "parents": ["page-home"],
  "properties": {
    "fillColor": [
      { "from": "#1783FF", "to": "#0066CC" },
      { "from": "#8B5CF6", "to": "#6366F1" }
    ],
    "fontFamily": [
      { "from": "Inter", "to": "Söhne" }
    ]
  }
}
```

---

## 典型工作流调用顺序

### 创建新设计

```
1. get_editor_state({ include_schema: true })   → 确认连接和当前文件
2. open_document({ filePathOrTemplate: "new" })  → 新建文件（如需要）
3. get_variables({ filePath })                   → 查看已有变量
4. set_variables({ filePath, variables: {...} }) → 设置设计 token
5. batch_design({ filePath, operations: "..." }) → 分批创建设计
   5a. 先建骨架 Frame
   5b. 再建 reusable 组件（含 Slots）
   5c. 用 C() 创建实例，M() 放置
6. get_screenshot({ filePath, nodeId })          → 渲染预览
7. snapshot_layout({ filePath, problemsOnly: true }) → 检查布局问题
```

### 盘点已有设计

```
1. batch_get({ filePath })                               → 顶层结构
2. batch_get({ filePath, patterns: [{ reusable: true }], readDepth: 2 }) → 列出组件
3. get_variables({ filePath })                            → 查看变量
4. search_all_unique_properties({ filePath, parents: [...], properties: [...] }) → 查看用了哪些颜色/字体
```

---

## 常见错误及修正

### 错误 1：copy 出来的实例直接改根属性

```
// ❌ 错误：直接改实例根属性会断开和原点的链接
U("instance-id", { fill: "#FF0000" })

// ✅ 正确：用 descendants 覆盖子元素
U("instance-id", {
  descendants: {
    "bg-rect": { fill: "#FF0000" }
  }
})
```

### 错误 2：layout 默认值搞错

```
// ❌ 错误：Frame 默认 layout 是 horizontal，不是 vertical
I(document, { type: "frame", name: "my-col", gap: 16 })

// ✅ 正确：竖向排列显式设置 layout: "vertical"
I(document, { type: "frame", name: "my-col", layout: "vertical", gap: 16 })
```

### 错误 3：icon_font 作为顶层元素

```
// ❌ 错误：icon_font 不能是顶层元素
I(document, { type: "icon_font", icon: "arrow-right" })

// ✅ 正确：icon_font 作为 frame 的子元素
btn = I(document, { type: "frame", w: 40, h: 40 })
I(btn, { type: "icon_font", iconFontFamily: "lucide", icon: "arrow-right", fontSize: 24 })
```

### 错误 4：slot 值不是数组

```
// ❌ 错误
I(comp, { type: "frame", slot: "text" })

// ✅ 正确
I(comp, { type: "frame", slot: ["text", "image"] })
```

### 错误 5：变量引用没加 $ 前缀

```
// ❌ 错误
U("title", { fill: "color-brand-primary" })

// ✅ 正确
U("title", { fill: "$color-brand-primary" })
```

### 错误 6：theme 变量 value 格式错误

```json
// ❌ 错误：value 直接是对象
{ "type": "color", "value": { "theme": { "mode": "light" }, "value": "#FFF" } }

// ✅ 正确：value 是数组，每项有 theme 和 value
{
  "type": "color",
  "value": [
    { "value": "#FFFFFF", "theme": { "mode": "light" } },
    { "value": "#1A1A1A", "theme": { "mode": "dark" } }
  ]
}
```

### 错误 7：set_variables 的 value 没有 type 字段

```json
// ❌ 错误：直接传裸值
{ "variables": { "color-brand": "#1783FF" } }

// ✅ 正确：必须包含 type 和 value
{ "variables": { "color-brand": { "type": "color", "value": "#1783FF" } } }
```

### 错误 8：get_screenshot 缺少 nodeId

```json
// ❌ 错误：没有指定渲染哪个节点
{ "filePath": "design.pen" }

// ✅ 正确：必须指定 nodeId
{ "filePath": "design.pen", "nodeId": "page-home" }
```

---

## 版本历史

| 版本 | 日期 | 变更内容 |
|------|------|---------|
| v1.0.0 | 2026-04-30 | 初始版本 |
