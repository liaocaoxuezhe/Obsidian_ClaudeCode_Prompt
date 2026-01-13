---
name: obsidian_base_use
description: 创建、配置和管理 Obsidian BASE 数据库，包括设置过滤器、属性、公式和视图。当用户提到 BASE、数据库、数据管理、项目管理或需要创建动态数据视图时使用此技能。
---

# obsidian_base_use

## Instructions
Provide clear, step-by-step guidance for Claude.

## BASE 数据库使用指南

### BASE 数据库概述

BASE 是 Obsidian 的数据库功能，允许您基于文件属性、标签和内容创建动态视图。

### 创建和嵌入 BASE

#### 创建方式
- **命令面板**："Bases: Create new base"
- **文件管理器**：右键文件夹 → "New base"
- **工具栏**：点击创建新base图标

#### 嵌入方式
- **嵌入文件**：`![[文件名.base]]`
- **嵌入代码块**：使用 `base` 代码块

### BASE 语法结构详解

#### 完整语法示例
```yaml
# 基础配置
name: "项目数据库"
description: "项目管理数据库"

# 过滤器 - 定义包含哪些文件
filters:
  and:
    - file.hasTag("项目")
    - file.inFolder("Projects")
    - file.name != "模板"

# 属性定义 - 显示哪些属性
properties:
  优先级:
    displayName: "优先级"
    type: "select"
    options: ["高", "中", "低"]
  截止日期:
    displayName: "截止日期"
    type: "date"
  进度:
    displayName: "进度"
    type: "number"
    min: 0
    max: 100

# 公式属性 - 动态计算属性
formulas:
  总价: "单价 * 数量"
  状态: 'if(进度 == 100, "✅ 完成", if(截止日期 < today(), "⚠️ 逾期", "🔄 进行中"))'
  剩余天数: '(截止日期 - today()).toFixed(0)'

# 视图配置 - 不同的展示方式
views:
  - type: table
    name: "项目表格"
    filters:
      and:
        - '优先级 == "高"'
    order:
      - 截止日期
      - file.name
    groupBy: 状态

  - type: kanban
    name: "项目看板"
    groupBy: 状态
    order:
      - 优先级

  - type: calendar
    name: "项目日历"
    dateProperty: 截止日期
```

### 属性类型详解

#### 笔记属性（Frontmatter）
- 存储在笔记 YAML frontmatter 中的属性
- 示例：`tags: [项目, 重要]`

#### 文件属性
- `file.name` - 文件名
- `file.path` - 文件路径
- `file.size` - 文件大小
- `file.ctime` - 创建时间
- `file.mtime` - 修改时间

#### 公式属性
- 基于其他属性计算的动态值
- 支持数学运算、字符串操作、条件判断

### 视图类型功能

#### 表格视图
- 类似 Excel 的表格展示
- 支持排序、筛选、分组
- 适合数据分析和概览

#### 卡片视图
- 卡片式布局
- 适合展示笔记摘要
- 支持自定义卡片模板

#### 日历视图
- 按日期组织笔记
- 适合时间线管理
- 支持拖拽调整日期

#### 看板视图
- 类似 Trello 的看板
- 适合项目管理
- 支持状态流转

### 过滤器功能详解

#### 基本过滤条件
```yaml
filters:
  and:
    - file.hasTag("项目")           # 包含特定标签
    - file.inFolder("Projects")     # 在特定文件夹
    - file.name.contains("重要")    # 文件名包含文本
    - '优先级 == "高"'              # 属性值等于
    - '进度 > 50'                   # 数值比较
    - '截止日期 > today()'          # 日期比较
```

#### 逻辑运算符
```yaml
filters:
  or:
    - file.hasTag("项目")
    - file.hasTag("任务")
  and:
    - file.inFolder("工作")
    - not: file.hasTag("已完成")
```

### 公式功能详解

#### 数学运算
```yaml
formulas:
  总价: "单价 * 数量"
  平均分: "(分数1 + 分数2 + 分数3) / 3"
  折扣价: "原价 * 0.8"
```

#### 字符串操作
```yaml
formulas:
  全名: "姓氏 + ' ' + 名字"
  状态: 'if(完成, "已完成", "进行中")'
  优先级显示: '"优先级：" + 优先级'
```

#### 日期处理
```yaml
formulas:
  剩余天数: '(截止日期 - today()).toFixed(0)'
  创建天数: '(today() - file.ctime).toFixed(0)'
  是否逾期: '截止日期 < today()'
```

### BASE 数据库创建流程

#### 1. 确定需求
- 明确要管理的数据类型
- 确定需要的视图和功能

#### 2. 设计结构
- 定义过滤器条件
- 设置属性字段
- 设计公式计算

#### 3. 创建 BASE
- 使用命令面板创建新 BASE
- 配置语法结构
- 测试功能完整性

#### 4. 嵌入使用
- 在相关笔记中嵌入 BASE
- 设置合适的显示位置

### 实用示例模板

#### 项目管理 BASE
```yaml
name: "项目管理数据库"

filters:
  and:
    - file.inFolder("Projects")

properties:
  优先级:
    displayName: "优先级"
    type: "select"
    options: ["高", "中", "低"]
  截止日期:
    displayName: "截止日期"
    type: "date"
  进度:
    displayName: "进度"
    type: "number"
    min: 0
    max: 100

formulas:
  状态: 'if(进度 == 100, "✅ 完成", if(截止日期 < today(), "⚠️ 逾期", "🔄 进行中"))'
  剩余天数: '(截止日期 - today()).toFixed(0)'

views:
  - type: table
    name: "项目概览"
    order:
      - 优先级
      - 截止日期

  - type: kanban
    name: "项目看板"
    groupBy: 状态
```

#### 读书笔记 BASE
```yaml
name: "读书笔记数据库"

filters:
  and:
    - file.hasTag("书籍")

properties:
  作者:
    displayName: "作者"
  总页数:
    displayName: "总页数"
    type: "number"
  已读页数:
    displayName: "已读页数"
    type: "number"
  开始日期:
    displayName: "开始日期"
    type: "date"

formulas:
  阅读进度: '(已读页数 / 总页数 * 100).toFixed(1)'
  状态: 'if(阅读进度 == 100, "✅ 已读完", "📖 阅读中")'

views:
  - type: table
    name: "阅读清单"
    filters:
      and:
        - '阅读进度 < 100'
    order:
      - 开始日期
```

#### 任务管理 BASE
```yaml
name: "任务管理数据库"

filters:
  and:
    - file.hasTag("任务")

properties:
  截止日期:
    displayName: "截止日期"
    type: "date"
  优先级:
    displayName: "优先级"
    type: "select"
    options: ["紧急", "重要", "一般"]
  状态:
    displayName: "状态"
    type: "select"
    options: ["待开始", "进行中", "已完成", "已取消"]

formulas:
  是否逾期: '截止日期 < today()'
  状态显示: 'if(状态 == "已完成", "✅ " + 状态, if(是否逾期, "⚠️ " + 状态, 状态))'

views:
  - type: kanban
    name: "任务看板"
    groupBy: 状态
    order:
      - 优先级
      - 截止日期
```

## Examples
Show concrete examples of using this Skill.