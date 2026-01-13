---
name: obsidian_templater_use
description: 创建和使用 Obsidian Templater 模板，包括动态变量、JavaScript 代码、系统命令和高级模板功能。当用户提到模板、Templater、动态内容、自动化笔记创建或需要模板语法帮助时使用此技能。
---

# obsidian_templater_use

## Instructions
Provide clear, step-by-step guidance for Claude.

## Templater 插件使用指南

### Templater 概述

Templater 是 Obsidian 的高级模板插件，支持动态变量、JavaScript 代码和系统命令，让模板更加智能和灵活。

### 基本模板变量

#### 核心变量
- `{{title}}` - 当前笔记标题
- `{{date}}` - 当前日期（默认格式：YYYY-MM-DD）
- `{{time}}` - 当前时间（默认格式：HH:mm）
- `{{date:YYYY年MM月DD日}}` - 自定义日期格式
- `{{time:HH时mm分}}` - 自定义时间格式

#### 基本模板示例
```markdown
---
title: {{title}}
created: {{date}} {{time}}
tags: [笔记]
---

# {{title}}

**创建时间**：{{date:YYYY年MM月DD日}} {{time:HH时mm分}}

## 内容

## 链接

## 总结
```

### Templater 高级功能

#### 动态变量系统
- `<% tp.file.title %>` - 当前文件标题
- `<% tp.file.creation_date("YYYY-MM-DD") %>` - 创建日期
- `<% tp.file.last_modified_date("YYYY-MM-DD") %>` - 最后修改日期
- `<% tp.date.now("YYYY-MM-DDTHH:mm") %>` - 当前日期时间
- `<% tp.date.weekday() %>` - 星期几
- `<% tp.date.now("YYYYMMDDHHmmssSSS") %>` - 时间戳UID

#### 系统交互功能
- `<% tp.system.prompt("请输入笔记主题") %>` - 用户输入提示
- `<% tp.system.suggester(["选项1", "选项2"], ["值1", "值2"]) %>` - 选择器
- `<% tp.system.clipboard() %>` - 获取剪贴板内容
- `<% tp.system.open("path/to/file") %>` - 打开文件

#### 文件操作功能
- `<% tp.file.cursor() %>` - 光标位置标记
- `<% tp.file.cursor(1) %>` - 多个光标位置
- `<% tp.file.path() %>` - 当前文件路径
- `<% tp.file.folder() %>` - 当前文件夹路径
- `<% tp.file.include("[[文件名]]") %>` - 包含其他文件内容

#### 高级数据处理
```javascript
<%*
// 随机标识符生成
const randomId = Math.floor(Math.random() * 10000000 + 80000000).toFixed(7)
tR += randomId

// 条件判断
const today = new Date()
const hour = today.getHours()
if (hour < 12) {
  tR += "上午好"
} else {
  tR += "下午好"
}

// 循环处理
const tags = ["工作", "学习", "生活"]
for (let tag of tags) {
  tR += `#${tag} `
}
%>
```

### 模板创建最佳实践

#### 1. 原子化设计
- 每个模板专注于特定类型的笔记
- 避免过于复杂的单个模板
- 保持模板的可重用性

#### 2. 标准化结构
- 保持一致的 frontmatter 格式
- 使用统一的变量命名规范
- 添加清晰的注释说明

#### 3. 灵活变量使用
- 结合基本变量和高级动态变量
- 使用条件判断适应不同场景
- 利用用户输入增加交互性

#### 4. 错误处理
- 添加默认值处理
- 使用 try-catch 处理异常
- 提供用户友好的错误提示

### 常用模板类型和示例

#### Zettelkasten 原子笔记模板
```markdown
---
uid: <% tp.date.now('YYYYMMDDHHmmssSSS') %>
created: <% tp.date.now('YYYY-MM-DD') %>
updated: <% tp.date.now('YYYY-MM-DD') %>
type: zettel
tags: []
---

# <% tp.file.title %>

<% tp.file.cursor() %>

## 相关链接

---
**创建时间**：<% tp.date.now('YYYY年MM月DD日 HH:mm') %>
```

#### 读书笔记模板
```markdown
---
title: <% tp.system.prompt("请输入书名") %>
author: <% tp.system.prompt("请输入作者") %>
created: <% tp.date.now('YYYY-MM-DD') %>
type: book-notes
tags: [读书笔记, <% tp.system.prompt("请输入分类") %>]
---

# <% tp.file.title %>

**作者**：<% tR += tp.system.prompt("请输入作者") %>
**页数**：<% tp.system.prompt("请输入总页数") %>
**阅读进度**：<% tp.system.prompt("当前阅读页数") %>/<% tp.system.prompt("总页数") %>
**评分**：⭐⭐⭐⭐⭐ (0-5)

## 核心观点

<% tp.file.cursor() %>

## 关键摘录

## 个人思考

## 行动计划

## 相关笔记

---
**创建时间**：<% tp.date.now('YYYY年MM月DD日 HH:mm') %>
```

#### 项目笔记模板
```markdown
---
project_name: <% tp.system.prompt("项目名称") %>
status: 进行中
priority: <% tp.system.suggester(["高", "中", "低"], ["high", "medium", "low"]) %>
created: <% tp.date.now('YYYY-MM-DD') %>
deadline: <% tp.system.prompt("截止日期 (YYYY-MM-DD)") %>
type: project
tags: [项目, <% tp.system.prompt("项目分类") %>]
---

# <% tp.file.title %>

**项目状态**：<% tp.system.suggester(["计划中", "进行中", "已暂停", "已完成"], ["planning", "in_progress", "paused", "completed"]) %>
**优先级**：<%* const priority = tp.system.suggester(["高", "中", "低"], ["high", "medium", "low"]); if (priority === "high") { tR += "🔴 高" } else if (priority === "medium") { tR += "🟡 中" } else { tR += "🟢 低" } %>
**截止日期**：<% tp.system.prompt("截止日期 (YYYY-MM-DD)") %>
**创建时间**：<% tp.date.now('YYYY-MM-DD') %>

## 项目目标

<% tp.file.cursor() %>

## 任务列表

- [ ] 任务1
- [ ] 任务2
- [ ] 任务3

## 进度记录

### <% tp.date.now('YYYY-MM-DD') %>

## 资源链接

## 遇到的问题

## 解决方案

## 总结反思

---
**创建时间**：<% tp.date.now('YYYY年MM月DD日 HH:mm') %>
```

#### 会议记录模板
```markdown
---
meeting_title: <% tp.system.prompt("会议主题") %>
meeting_date: <% tp.date.now('YYYY-MM-DD') %>
meeting_time: <% tp.system.prompt("会议时间") %>
attendees: <% tp.system.prompt("参会人员 (用逗号分隔)") %>
type: meeting-notes
tags: [会议记录]
---

# <% tp.file.title %>

**会议时间**：<% tp.date.now('YYYY-MM-DD') %> <% tp.system.prompt("会议时间") %>
**参会人员**：<% tp.system.prompt("参会人员 (用逗号分隔)") %>
**记录人**：<% tp.system.prompt("记录人") %>

## 会议议程

1. <% tp.file.cursor() %>
2.
3.

## 讨论要点

### 议题1：议题名称
**讨论内容**：
**决策结果**：

### 议题2：议题名称
**讨论内容**：
**决策结果**：

## 行动项

| 任务 | 负责人 | 截止日期 | 状态 |
|------|--------|----------|------|
| 任务1 |  |  | 待开始 |
| 任务2 |  |  | 待开始 |

## 下次会议安排

**时间**：
**议题**：

## 附件链接

---
**记录时间**：<% tp.date.now('YYYY年MM月DD日 HH:mm') %>
```

#### 日记模板
```markdown
---
date: <% tp.date.now('YYYY-MM-DD') %>
weekday: <% tp.date.weekday() %>
weather: <% tp.system.prompt("今天天气如何？") %>
mood: <% tp.system.suggester(["😊 很好", "😐 一般", "😔 不好"], ["good", "normal", "bad"]) %>
type: daily
tags: [日记]
---

# <% tp.date.now('YYYY年MM月DD日') %> <% tp.date.weekday() %>

## 今日概要

**天气**：<% tp.system.prompt("今天天气如何？") %>
**心情**：<%* const mood = tp.system.suggester(["😊 很好", "😐 一般", "😔 不好"], ["good", "normal", "bad"]); tR += mood %>

## 专注时刻

<% tp.file.cursor() %>

## 学习收获

## 生活感悟

## 明日计划

## 感恩记录

---
**记录时间**：<% tp.date.now('YYYY年MM月DD日 HH:mm') %>
```

### 高级模板技巧

#### 动态文件名生成
```javascript
<%*
// 根据当前日期和时间生成文件名
const timestamp = tp.date.now('YYYYMMDDHHmmss')
const title = tp.system.prompt("请输入笔记标题")
await tp.file.rename(`${timestamp}-${title}`)
%>
```

#### 自动标签管理
```javascript
<%*
// 根据文件夹自动添加标签
const folder = tp.file.folder()
const tags = []
if (folder.includes("Projects")) {
  tags.push("项目")
}
if (folder.includes("Books")) {
  tags.push("读书")
}
tags.push(tp.system.prompt("请输入其他标签 (用空格分隔)"))
tR += tags.join(" ")
%>
```

#### 智能链接创建
```javascript
<%*
// 检查相关文件是否存在并创建链接
const relatedFile = tp.system.prompt("请输入相关笔记名称")
const files = app.vault.getFiles()
const exists = files.some(file => file.basename === relatedFile)
if (exists) {
  tR += `[[${relatedFile}]]`
} else {
  tR += relatedFile + " (待创建)"
}
%>
```

### 模板使用流程

#### 1. 安装和配置
- 从社区插件市场安装 Templater
- 配置模板文件夹路径
- 启用自动触发器（可选）

#### 2. 创建模板
- 在 `/template/` 文件夹创建新模板
- 使用 `.md` 扩展名
- 编写模板内容，包含必要的变量

#### 3. 应用模板
- 使用命令面板："Templater: Insert Template"
- 设置快捷键快速访问
- 配置文件夹自动触发器

#### 4. 调试和优化
- 检查模板语法错误
- 测试动态变量功能
- 根据使用情况调整模板结构

### 常见问题解决

#### 语法错误
- 检查 `<% %>` 标签配对
- 确认 JavaScript 语法正确
- 验证变量名拼写

#### 性能优化
- 避免过于复杂的循环
- 减少文件系统操作
- 使用缓存机制

#### 兼容性问题
- 确保插件版本兼容
- 检查与其他插件的冲突
- 备份重要模板

## Examples
Show concrete examples of using this Skill.