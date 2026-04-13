# `src/slides/markup.ts` 总结

[`src/slides/markup.ts`](/C:/Users/Haruta/Documents/code/Web/working-with-ai/src/slides/markup.ts) 是整个 slides 项目的内容中心。它不负责行为逻辑，也不负责样式细节，而是集中定义了所有页面的 HTML 结构、页面顺序、fragment 节点以及各类 demo 依赖的 DOM 标记。

## 文件职责

这份文件主要负责三件事：

- 定义所有 slide 的顺序和页面内容
- 为 `slide-deck.ts` 提供 `data-f` fragment 结构
- 为各个 demo 模块提供稳定的 `id`、`class` 和 `data-*` 钩子

可以把它理解成整套演示文稿的“内容模板文件”。

## 导出方式

文件通过一个大模板字符串导出：

- 导出变量：`slidesMarkup`
- 类型：`String.raw` 包裹的 HTML 字符串

这个字符串会在 [`src/main.ts`](/C:/Users/Haruta/Documents/code/Web/working-with-ai/src/main.ts) 中被注入到 `#app`。

## 页面结构概览

当前 `markup.ts` 中定义的页面大致如下：

1. 标题页 `Working with AI`
2. `Tab → Agent`
3. `Agent / Prompt / Context`
4. `AI 编程的自动驾驶等级`
5. `我的工具选择`
6. `什么是 Skills?`
7. `CLI vs MCP`
8. `我的 Workflow`
9. `Research 双模式`
10. `一些个人 Prompt 经验`
11. `不只是 Vibe Coding` 章节封面
12. 设计交互协作
13. 会议讨论准备
14. 辅助代码理解
15. 用 AI 做设计
16. `一些变化` 章节封面
17. 信息获取方式在变
18. 默认不信任“快”
19. AI 在生活中
20. Emoji 猜影视作品
21. `Q & A`

说明：

- 文件中还有一页 `knowledge.jpeg` 的旧内容，但当前是注释状态，没有参与实际渲染。

## 重要结构约定

### 1. 每页都用 `<section class="slide">`

每一页都是一个 `.slide` 容器。

这直接被 [`src/slide-deck.ts`](/C:/Users/Haruta/Documents/code/Web/working-with-ai/src/slide-deck.ts) 用来：

- 收集全部页面
- 管理当前激活页
- 做翻页动画
- 构建 overview 缩略图

### 2. `data-f` 控制 fragment

文件里大量使用 `data-f="1"`, `data-f="2"` 这类标记。

它们的作用是：

- 标记某个元素属于第几步 fragment
- 由 `slide-deck.ts` 统一切换 `.f-visible`
- 控制同一页内部的逐步出现

所以改页面时，如果你要保留“逐步讲解”的节奏，必须同时考虑 `data-f` 编号。

### 3. demo 依赖明确的 DOM 钩子

很多页面不是纯静态文案，而是带交互或动画，所以 `markup.ts` 里还承担了“给脚本留挂点”的职责。

典型例子：

- `Prompt / Context / Agent` 页
  - `data-concept-demo`
  - `data-step`
  - `.prompt-goal`
  - `.context-core`
  - `.agent-progress-fill`
- Emoji 游戏页
  - `#emoji-display`
  - `#emoji-answer`
  - `#emoji-start`
  - `#emoji-reveal`
  - `#emoji-next`
  - `#copy-prompt`
- AI taste 页
  - `#ai-iframe`
  - `.taste-btn`
- JPEG demo 页（当前注释）
  - `#jpeg-canvas`
  - `#jpeg-slider`
  - `#jpeg-quality-label`
- overlay 预览
  - `.clickable[data-url]`
  - `data-mobile="true"`

这些钩子一旦改名，相关 TS 模块就会失效。

## 按页面主题归类

### 开场与框架页

包括：

- 标题页
- `Tab → Agent`
- `Agent / Prompt / Context`
- 自动驾驶等级

这一组页面负责建立整场分享的基础框架：

- AI 工具正在从补全演进到 Agent
- `Prompt / Context / Agent` 是三个核心变量
- 自动化程度会不断升高

### 方法与工具页

包括：

- 工具选择
- `Skills`
- `CLI vs MCP`
- `Workflow`
- `Research 双模式`
- `Prompt 经验`

这一组页面更偏“怎么做”：

- 用什么工具
- Skill 的定位是什么
- CLI 和 MCP 如何分工
- 实际 workflow 长什么样
- 调研和 prompt 应该怎么用

### 应用场景页

包括：

- 不只是 Vibe Coding 章节
- 设计交互协作
- 会议讨论准备
- 辅助代码理解
- 用 AI 做设计

这一组页面在说明：AI 不只是在写代码，也在改变沟通、设计和协作方式。

### 观点与感受页

包括：

- 一些变化
- 信息获取方式在变
- 默认不信任“快”
- AI 在生活中

这一组页面更偏作者自己的判断与习惯变化。

### 互动收尾页

包括：

- Emoji 猜影视作品
- Q&A

负责把演示从讲述切到互动和收束。

## 当前文件里最关键的几页

如果后续还会继续迭代，我认为 `markup.ts` 里最关键的是下面几页：

### `Tab → Agent`

因为这是整场分享最早建立叙事方向的地方。

### `Agent / Prompt / Context`

因为这是概念解释页，而且带动画，后续最容易继续改。

### `什么是 Skills?`

因为这是术语解释页，直接决定观众是否能理解后面的 `CLI vs MCP`。

### `我的 Workflow`

因为这是最贴近个人实践的一页，属于内容重点。

### `用 AI 做设计`

因为它承担了“AI 味”这个比较有辨识度的话题。

## 修改这份文件时要注意什么

### 1. 不要只改文案，不看脚本依赖

如果一个元素带 `id`、`data-step`、`data-concept-demo`、`data-url` 之类标记，它很可能已经被脚本引用。

### 2. 不要随意打乱 `data-f`

这会直接影响讲述节奏，甚至让翻页和步进逻辑变得不自然。

### 3. 注意编码

这个文件体积大、中文多、结构密，一旦被错误编码污染，很容易出现整页乱码。

### 4. 控制复杂度

如果后面某一页的结构越来越复杂，应该考虑继续拆分，不要让 `markup.ts` 无限膨胀。

## 一句话总结

`src/slides/markup.ts` 不是普通的 HTML 字符串文件，它实际上是整个 slides 的内容编排层：定义页面、组织叙事、安排 fragment 节奏，并作为所有交互 demo 的 DOM 基座。
