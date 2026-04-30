# MCP 工具完整参数参考 · Pencil 版

> 基于 Pencil 官方文档整理。**所有示例均为实际 JSON 调用格式**，可直接复制使用。

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

---

## batch_design

**用途**：创建、修改、复制、移动、删除设计元素；生成和放置图片

### operations[] — 操作数组

所有操作在同一个 `batch_design` call 内按顺序执行。

#### type: "create" — 创建元素

```json
{
  "operations": [
    {
      "type": "create",
      "element": {
        "id": "my-frame",
        "type": "frame",
        "x": 0,
        "y": 0,
        "width": 1440,
        "height": 800,
        "fill": "#F8F7F4",
        "layout": "vertical",
        "gap": 24
      }
    }
  ]
}
```

**element.type 支持的值**：

| type | 说明 |
|------|------|
| `frame` | 容器，可设置 flex layout |
| `rectangle` | 矩形色块 |
| `text` | 文本 |
| `image` | 图片（需先导入获得内部引用） |
| `icon_font` | 图标（需作为子元素存在） |
| `group` | 编组 |

**Frame layout 属性**：

| 属性 | 值 | 说明 |
|------|----|------|
| `layout` | `"vertical"` / `"horizontal"` / `"none"` | flex 布局方向。**Frame 默认是 horizontal**，Group 默认是 none |
| `gap` | number | 子元素间距 |
| `padding` | number / [h, v] / [t, r, b, l] | 内边距 |
| `justifyContent` | `"start"` / `"center"` / `"end"` / `"space_between"` / `"space_around"` | 主轴对齐 |
| `alignItems` | `"start"` / `"center"` / `"end"` / `"stretch"` | 交叉轴对齐 |

**text 元素属性**：

| 属性 | 值 | 说明 |
|------|----|------|
| `content` | string | 文本内容 |
| `fontSize` | number | 字号（px） |
| `fontFamily` | string | 字体名（如 "Newsreader"、"Inter"） |
| `fontWeight` | number | 字重（400/500/700） |
| `lineHeight` | number | 行高（建议 1.2-1.7） |
| `textAlign` | `"left"` / `"center"` / `"right"` | 文本对齐 |

**创建 reusable 组件（组件原点）**：

```json
{
  "type": "create",
  "element": {
    "id": "button-primary",
    "type": "frame",
    "width": 120,
    "height": 40,
    "fill": "#1783FF",
    "reusable": true
  }
}
```

**创建带 slot 的组件**：

```json
{
  "type": "create",
  "element": {
    "id": "card-component",
    "type": "frame",
    "width": 320,
    "height": 200,
    "reusable": true,
    "children": [
      {
        "id": "card-header",
        "type": "frame",
        "fill": "#F0F0F0",
        "height": 40
      },
      {
        "id": "card-content-slot",
        "type": "frame",
        "fill": "#FFFFFF",
        "slot": ["text", "image"]
      },
      {
        "id": "card-footer",
        "type": "frame",
        "fill": "#F0F0F0",
        "height": 40
      }
    ]
  }
}
```

**创建带 icon 的按钮组件**：

```json
{
  "type": "create",
  "element": {
    "id": "icon-button",
    "type": "frame",
    "width": 120,
    "height": 40,
    "fill": "#1783FF",
    "reusable": true,
    "children": [
      {
        "id": "btn-icon",
        "type": "icon_font",
        "iconFontFamily": "lucide",
        "icon": "arrow-right",
        "fontSize": 18,
        "fill": "#FFFFFF"
      },
      {
        "id": "btn-label",
        "type": "text",
        "content": "Submit",
        "fontSize": 14,
        "fontFamily": "Inter",
        "fontWeight": 500,
        "fill": "#FFFFFF"
      }
    ]
  }
}
```

---

#### type: "insert" — 插入子元素

```json
{ "type": "insert", "parent": "hero-section", "child": "hero-title" }
```

| 参数 | 类型 | 说明 |
|------|------|------|
| `parent` | string | 父元素 ID |
| `child` | string | 子元素 ID |
| `index` | number | （可选）插入位置，默认追加到末尾 |

**注意**：父子关系建立后，父元素的 `layout` 属性决定子元素的排列方式。

---

#### type: "copy" — 复制组件创建实例

```json
{
  "operations": [
    {
      "type": "copy",
      "sourceId": "button-primary",
      "newId": "button-secondary",
      "x": 140,
      "y": 0
    }
  ]
}
```

**copy 是创建 instance 的方式**：
- 复制出的实例默认继承原点的所有属性
- 实例在 Pencil 内部用 `type: "ref"` 表示（不要自己构造 ref）
- 复制后可以用 `update` + `descendants` 定制

**用 descendants 定制实例**：

```json
{
  "operations": [
    {
      "type": "copy",
      "sourceId": "card-component",
      "newId": "article-card",
      "x": 0,
      "y": 0
    },
    {
      "type": "update",
      "id": "article-card",
      "changes": {
        "descendants": {
          "card-header": {
            "fill": "#1A1A1A"
          },
          "card-content-slot": {
            "children": [
              {
                "id": "slot-image",
                "type": "image",
                "src": "internal-ref-to-image"
              }
            ]
          }
        }
      }
    }
  ]
}
```

---

#### type: "update" — 修改属性或定制实例

```json
{
  "operations": [
    {
      "type": "update",
      "id": "hero-title",
      "changes": {
        "fontSize": 96,
        "fill": "#1783FF"
      }
    }
  ]
}
```

**定制实例时用 `descendants`**：

```json
{
  "type": "update",
  "id": "instance-id",
  "changes": {
    "descendants": {
      "child-element-id": {
        "text": "New content",
        "fill": "#FF0000"
      }
    }
  }
}
```

---

#### type: "move" — 移动元素

```json
{
  "operations": [
    {
      "type": "move",
      "id": "my-element",
      "parent": "new-parent",
      "index": 0
    }
  ]
}
```

---

#### type: "delete" — 删除元素

```json
{
  "operations": [
    { "type": "delete", "id": "unwanted-element" }
  ]
}
```

---

#### type: "replace" — 替换元素

```json
{
  "operations": [
    {
      "type": "replace",
      "id": "old-element",
      "newElement": {
        "id": "new-element",
        "type": "frame",
        "width": 200,
        "height": 100,
        "fill": "#F8F7F4"
      }
    }
  ]
}
```

---

### 完整调用示例

```json
{
  "operations": [
    {
      "type": "create",
      "element": {
        "id": "hero-section",
        "type": "frame",
        "width": 1440,
        "height": 800,
        "fill": "#F8F7F4",
        "layout": "vertical",
        "gap": 24
      }
    },
    {
      "type": "create",
      "element": {
        "id": "hero-title",
        "type": "text",
        "content": "Design that ships.",
        "fontSize": 72,
        "fontFamily": "Newsreader",
        "fontWeight": 700,
        "fill": "#1A1A1A",
        "lineHeight": 1.1
      }
    },
    {
      "type": "create",
      "element": {
        "id": "cta-button",
        "type": "frame",
        "width": 160,
        "height": 48,
        "fill": "#1783FF",
        "reusable": true
      }
    },
    { "type": "insert", "parent": "hero-section", "child": "hero-title" },
    { "type": "insert", "parent": "hero-section", "child": "cta-button" }
  ]
}
```

---

## batch_get

**用途**：读取设计结构和内容

### action: "get_tree" — 获取完整文档树

```json
{ "action": "get_tree" }
```

**返回**：
```json
{
  "children": [
    {
      "id": "canvas-root",
      "type": "frame",
      "children": [...]
    }
  ]
}
```

### action: "get" — 获取元素详情

```json
{ "action": "get", "id": "hero-section" }
```

### action: "search" — 搜索元素

```json
{
  "action": "search",
  "pattern": {
    "type": "frame",
    "reusable": true
  },
  "limit": 50
}
```

**pattern 支持的字段**：

| 字段 | 类型 | 说明 |
|------|------|------|
| `type` | string | 元素类型（frame/rectangle/text/image/icon_font...） |
| `id` | string / RegExp | 元素 ID |
| `reusable` | boolean | 是否为 reusable 组件 |
| `fill` | string | 精确匹配填充色 |

### action: "list_components" — 列出所有 reusable 组件

```json
{ "action": "list_components" }
```

### action: "get_component" — 获取组件定义

```json
{ "action": "get_component", "id": "button-primary" }
```

---

## get_screenshot

**用途**：渲染设计预览

### 参数

| 参数 | 类型 | 说明 |
|------|------|------|
| `full_page` | boolean | true = 渲染整个画布 |
| `frame` | string | 渲染特定 frame ID |
| `scale` | number | 缩放比例，默认 1 |
| `format` | `"png"` / `"jpg"` | 输出格式，默认 png |

### 调用

```json
{ "full_page": true }
```

```json
{ "frame": "hero-section", "scale": 2 }
```

```json
{ "compare": true }
```

---

## snapshot_layout

**用途**：分析布局问题

### action: "analyze" — 分析布局结构

```json
{ "action": "analyze" }
```

或指定 frame：

```json
{ "action": "analyze", "frame": "main-content" }
```

**返回**：
```json
{
  "elements": [
    {
      "id": "hero-section",
      "type": "frame",
      "x": 0,
      "y": 0,
      "width": 1440,
      "height": 800,
      "children": 3
    }
  ],
  "issues": [
    {
      "type": "overlap",
      "elements": ["element-a", "element-b"],
      "description": "Elements are overlapping"
    },
    {
      "type": "misalign",
      "elements": ["element-c", "element-d"],
      "description": "Elements not aligned to 8px grid"
    }
  ]
}
```

### action: "find_overlaps" — 查找重叠元素

```json
{ "action": "find_overlaps" }
```

### action: "check_alignment" — 检查网格对齐

```json
{ "action": "check_alignment", "grid": 8 }
```

---

## get_variables

**用途**：读取当前文档的所有变量

### 调用

```json
{}
```

**返回**：
```json
{
  "variables": {
    "color.brand-primary": {
      "type": "color",
      "value": "#1783FF"
    },
    "spacing.base": {
      "type": "number",
      "value": 8
    },
    "font.display": {
      "type": "string",
      "value": "Newsreader"
    }
  },
  "themes": {
    "mode": ["light", "dark"],
    "spacing": ["regular", "condensed"]
  }
}
```

---

## set_variables

**用途**：设置/更新变量

### 简单变量（无主题）

```json
{
  "variables": {
    "color.brand-primary": {
      "type": "color",
      "value": "#1783FF"
    },
    "color.brand-bg": {
      "type": "color",
      "value": "#FFFFFF"
    },
    "spacing.base": {
      "type": "number",
      "value": 8
    },
    "font.display": {
      "type": "string",
      "value": "Newsreader"
    }
  }
}
```

### 主题感知变量（带暗色模式）

```json
{
  "variables": {
    "color.brand-primary": {
      "type": "color",
      "value": [
        { "value": "#1783FF", "theme": { "mode": "light" }},
        { "value": "#60A5FA", "theme": { "mode": "dark" }}
      ]
    },
    "color.brand-bg": {
      "type": "color",
      "value": [
        { "value": "#FFFFFF", "theme": { "mode": "light" }},
        { "value": "#1A1A1A", "theme": { "mode": "dark" }}
      ]
    }
  }
}
```

---

## get_editor_state

**用途**：获取当前编辑器状态

### 调用

```json
{}
```

**返回**：
```json
{
  "active_file": "/path/to/design.pen",
  "selection": ["hero-section"],
  "zoom": 1,
  "viewport": { "x": 0, "y": 0 }
}
```

---

## 工具调用顺序建议

### 典型创建设计流程

```
1. get_editor_state()                      → 确认当前文件
2. batch_get({ action: "get_tree" })     → 了解文档结构
3. batch_get({ action: "list_components" }) → 盘点已有组件
4. get_variables()                         → 查看已有变量
5. set_variables({ variables: {...} })     → 设置品牌变量
6. batch_design({ operations: [...] })    → 创建设计（分批）
   6a. 先建骨架 Frame
   6b. 再建 reusable 组件（含 Slots）
   6c. 用 copy 创建实例
7. get_screenshot({ full_page: true })   → 渲染预览
8. snapshot_layout({ action: "analyze" }) → 检查布局问题
```

### 典型组件实例化流程

```
1. batch_get({ action: "get_component", id: "button-primary" }) → 查看组件定义
2. batch_design({
     operations: [
       { type: "copy", sourceId: "button-primary", newId: "btn-1", x: 0, y: 0 },
       { type: "copy", sourceId: "button-primary", newId: "btn-2", x: 100, y: 0 }
     ]
   })                                       → 创建多个实例
3. batch_design({
     operations: [
       { type: "update", id: "btn-2", changes: { descendants: { "label": { text: "Cancel" }}}}
     ]
   })                                       → 定制实例
```

---

## 常见错误及修正

### 错误 1：copy 出来的实例直接改根属性

```json
// ❌ 错误：直接改实例根属性会断开和原点的链接
{
  "type": "update",
  "id": "instance-id",
  "changes": { "fill": "#FF0000" }
}

// ✅ 正确：先看组件定义里根属性的子元素 ID，然后用 descendants 覆盖
{
  "type": "update",
  "id": "instance-id",
  "changes": {
    "descendants": {
      "bg-rect": { "fill": "#FF0000" }
    }
  }
}
```

### 错误 2：layout 属性默认值搞错

```json
// ❌ 错误：Frame 默认 layout 是 horizontal，不是 none
{ "element": { "id": "my-col", "type": "frame", "gap": 16 }}
// 如果想要竖向排列但没设置 layout，效果可能不对

// ✅ 正确：竖向排列显式设置 layout: "vertical"
{ "element": { "id": "my-col", "type": "frame", "layout": "vertical", "gap": 16 }}
```

### 错误 3：insert 时元素还没创建

```json
// ❌ 错误：child 元素还没创建
{ "type": "insert", "parent": "frame-a", "child": "title-a" }

// ✅ 正确：先 create，再 insert
{
  "operations": [
    { "type": "create", "element": { "id": "title-a", ... }},
    { "type": "insert", "parent": "frame-a", "child": "title-a" }
  ]
}
```

### 错误 4：slot 值不是数组

```json
// ❌ 错误：slot 的值应该是数组
{ "id": "my-slot", "type": "frame", "slot": "text" }

// ✅ 正确：slot 是允许的组件类型数组
{ "id": "my-slot", "type": "frame", "slot": ["text", "image"] }
```

### 错误 5：变量引用没加 $ 前缀

```json
// ❌ 错误：直接写变量名
{ "fill": "color.brand-primary" }

// ✅ 正确：变量名前加 $ 前缀
{ "fill": "$color.brand-primary" }
```

### 错误 6：icon_font 没有作为子元素存在

icon_font 不是顶层元素，需要包装在 frame 里：

```json
// ❌ 错误：把 icon_font 作为顶层元素
{ "type": "create", "element": { "id": "icon", "type": "icon_font", "icon": "arrow-right" }}

// ✅ 正确：icon_font 作为 frame 的子元素
{ "type": "create", "element": {
  "id": "icon-wrapper",
  "type": "frame",
  "width": 24,
  "height": 24,
  "children": [
    { "id": "icon", "type": "icon_font", "iconFontFamily": "lucide", "icon": "arrow-right", "fontSize": 24 }
  ]
}}
```

### 错误 7：theme 变量 value 格式错误

```json
// ❌ 错误：value 直接是对象
{ "type": "color", "value": { "theme": { "mode": "light" }, "value": "#FFF" }}

// ✅ 正确：value 是数组，每项有 theme 和 value
{ "type": "color", "value": [
    { "value": "#FFFFFF", "theme": { "mode": "light" }},
    { "value": "#1A1A1A", "theme": { "mode": "dark" }}
  ]
}
```

---

## Multi-fill / Multi-effect 说明

Pencil 支持元素的多种填充和效果叠加，但通过 MCP 操作时要注意：

### Multi-fill（多层填充）

一个元素可以有多个 fill，按顺序叠加：

```json
{
  "id": "glass-card",
  "type": "frame",
  "fill": [
    { "type": "color", "color": "#FFFFFF", "opacity": 0.1 },
    { "type": "linear", "from": "#FFFFFF", "to": "#E5E5E5", "angle": 90 }
  ]
}
```

**常见场景**：玻璃拟态卡片（半透明 + 模糊效果）

### Multi-effect（多层效果）

一个元素可以有多个 effect：

```json
{
  "id": "shadow-card",
  "type": "frame",
  "effect": [
    { "type": "drop-shadow", "color": "#000000", "opacity": 0.15, "blur": 10, "offset": [0, 4] },
    { "type": "inner-shadow", "color": "#FFFFFF", "opacity": 0.1, "blur": 5 }
  ]
}
```

**注意**：MCP batch_design 对 multi-fill/multi-effect 的支持程度需要实测，以上是 .pen 格式规范，实际调用可能有差异。

---

## Image 元素说明

### 图片来源

Pencil 中使用图片有几种方式：

1. **通过 Pencil UI 导入**：在 Pencil 里手动导入图片，获得内部引用
2. **通过 Clipboard 粘贴**：复制图片到剪贴板，在 Pencil 里粘贴
3. **通过 URL 引用**：某些情况下可以直接用外部 URL

### image 元素结构

```json
{
  "id": "hero-image",
  "type": "image",
  "src": "internal-ref-or-base64",   // 内部引用或 base64
  "width": 1440,
  "height": 800,
  "objectFit": "cover"
}
```

**建议**：品牌资产协议中获取的图片，建议通过 Pencil UI 导入并记录其内部 ID，而不是尝试通过 MCP 传递外部 URL。

---

## 版本历史

| 版本 | 日期 | 变更内容 |
|------|------|---------|
| v1.0.0 | 2026-04-30 | 初始版本 |
