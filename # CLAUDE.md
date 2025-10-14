# CLAUDE.md
此文件为 Claude Code (claude.ai/code) 在处理此存储库中的代码时提供指导。
## 概述
这是一个 Obsidian 知识管理库，包含研究笔记、文章和个人知识库内容。该库采用受 Zettelkasten 启发的组织方式，并包含各种插件以增强功能。
## Obsidian 配置
### 启用的核心插件：
- 文件浏览器
- 全局搜索
- 反向链接面板
- 标签面板
- 日记
- 模板
- 命令面板
- 大纲
- 字数统计
- 画布
- 属性
### 安装的社区插件：
- **copilot**：AI 辅助笔记
- **calendar**：日记日历视图
## 目录结构
- `/Clippings/`：保存的网页内容和文章
- `/Excalidraw/`：图表和视觉笔记
- `/Zettelkasten/`：卡片和笔记法中的 zettelkasten，用来存放原子笔记
- `/template/`：笔记模板
- `/Diary/`：Obsidian 的每日文件
- `/Create/`：维护创作的内容
- `/BASE/`：存放了一些现有的 BASE 数据库
## 常见任务
### 任务类型一、笔记创建流程
#### 1. 选择模板
- 使用 `/template/` 目录中的模板
- 根据笔记类型选择合适的模板（读书笔记、项目笔记等）
#### 2. 创建原子化笔记
- 遵循 Zettelkasten 原则：每则笔记一个核心想法
- 保持笔记简洁，聚焦单一主题
- 使用有意义的文件名
#### 3. 元数据管理
- 利用 frontmatter 属性进行元数据管理
- 标准属性：`title`, `created`, `tags`, `aliases`
- 自定义属性根据笔记类型添加
#### 4. 内容编写
- 使用 Markdown 语法
- 保持结构清晰：标题、列表、引用等
- 添加相关链接和引用
### 任务类型二、链接笔记系统
Obsidian 的核心功能是链接笔记，创建知识网络：
！！！重要提醒：如果你要关联文档，请先使用本地搜索工具搜索文档的名称，并确保这个文档存在，非常重要！！
#### 基本链接语法
- `[[笔记名称]]` - 创建内部链接
- `[[笔记|显示文本]]` - 自定义链接显示文本
- `[[笔记#标题]]` - 链接到特定标题
- `[[笔记#^块标识]]` - 链接到特定文本块
#### 高级链接功能
- `![[文件]]` - 嵌入文件内容（图片、PDF等）
- `[[^^搜索文本]]` - 搜索并链接到包含特定文本的块
- `[[##标题]]` - 搜索并链接到包含特定文本的标题
#### 链接最佳实践
1. **双向链接**：创建有意义的双向关系
2. **上下文链接**：在相关上下文中添加链接
3. **标签系统**：使用标签进行分类和组织
4. **反向链接**：利用反向链接面板发现新关联
### 搜索文件或文档
1. 当你需要在 Obsidian 文件夹里检索文件时，使用 fd 和 rg 的命令行来检索文件；
2. 如果你要创建一个新的文档链接（像[[]]），请先搜索想要关联的文档，并确保这个文档存在。如果不存在，则不要关联。
3. Obsidian 的很多文件都有 YAML 前置参数(Frontmatter)，这些内容应该也可以被检索；
### 任务类型三、BASE 数据库创建流程
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
## 模板使用说明
### 模板系统概述
Obsidian 支持两种模板系统：
- **核心模板插件**：基础模板功能
- **Templater 插件**：高级动态模板功能
### 模板文件位置
- 模板文件存储在 `/template/` 目录中
- 支持 `.md` 文件格式
### 核心模板插件使用
#### 基本模板变量
- `{{title}}` - 当前笔记标题
- `{{date}}` - 当前日期（默认格式：YYYY-MM-DD）
- `{{time}}` - 当前时间（默认格式：HH:mm）
- `{{date:YYYY年MM月DD日}}` - 自定义日期格式
- `{{time:HH时mm分}}` - 自定义时间格式
#### 模板示例
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
### Templater 插件高级功能
#### 动态变量
- `<% tp.file.title %>` - 当前文件标题
- `<% tp.file.creation_date("YYYY-MM-DD") %>` - 创建日期
- `<% tp.file.last_modified_date("YYYY-MM-DD") %>` - 最后修改日期
#### 系统命令
- `<% tp.system.prompt("请输入笔记主题") %>` - 用户输入提示
- `<% tp.system.suggester(["选项1", "选项2"], ["值1", "值2"]) %>` - 选择器
#### 文件操作
- `<% tp.file.cursor() %>` - 光标位置标记
- `<% tp.file.cursor(1) %>` - 多个光标位置
### 模板创建最佳实践
1. **原子化设计**：每个模板专注于特定类型的笔记
2. **标准化结构**：保持一致的 frontmatter 和内容结构
3. **灵活变量**：使用动态变量适应不同场景
4. **清晰注释**：在模板中添加使用说明
## Base 数据库说明
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
filters:  
  and:  
   - file.hasTag("项目")      # 包含特定标签  
   - file.inFolder("Projects")   # 在特定文件夹  
   - file.name.contains("重要")  # 文件名包含文本  
   - '优先级 == "高"'       # 属性值等于  
   - '进度 > 50'          # 数值比较  
   - '截止日期 > today()'     # 日期比较
#### 逻辑运算符
filters:  
  or:  
   - file.hasTag("项目")  
   - file.hasTag("任务")  
  and:  
   - file.inFolder("工作")  
   - not: file.hasTag("已完成")
### 公式功能详解
#### 数学运算
formulas:  
  总价: "单价 * 数量"  
  平均分: "(分数1 + 分数2 + 分数3) / 3"  
  折扣价: "原价 * 0.8"
#### 字符串操作
formulas:  
  全名: "姓氏 + ' ' + 名字"  
  状态: 'if(完成, "已完成", "进行中")'  
  优先级显示: '"优先级：" + 优先级'
#### 日期处理
formulas:  
  剩余天数: '(截止日期 - today()).toFixed(0)'  
  创建天数: '(today() - file.ctime).toFixed(0)'  
  是否逾期: '截止日期 < today()'
### 实用示例模板
#### 现有 BASE 数据库配置
##### Zettelkasten_base.base 特点
- 使用公式生成随时间变化的随机数用于随机排序
- 支持表格和卡片两种视图
- 自动过滤 Zettelkasten 文件夹中的 Markdown 文件
- 支持按创建时间、文件名、标签等多维度排序
##### 公式示例
formulas:  
  随时间变化的随机数: (( (number(this.时间戳 UID) * 100011979 + 500067713) % 900066731 ) * ( (now().minute * 800067089 + 800068411) % 800053967 ) + 900067309) % 900066571
#### 项目管理 BASE
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
#### 读书笔记 BASE
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
#### 任务管理 BASE
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
## 开发说明
该库不包含传统的软件开发代码。主要内容是用于知识管理的 Markdown 文件。任何开发工作通常涉及：
- 创建或修改 Obsidian Base 数据库
- 创建或修改 Template 文件
- 按照用户的要求写作内容和处理文档
### 常用命令和工具
#### 文件搜索命令
- `fd "搜索内容"` - 使用 fd 工具搜索文件名
- `rg "搜索内容"` - 使用 ripgrep 搜索文件内容
- `fd -e md "关键词"` - 搜索特定扩展名的文件
- `rg -i "关键词" --type md` - 不区分大小写搜索 Markdown 文件
#### 文件管理
- 使用 `fd` 和 `rg` 进行高效文件检索
- 在创建文档链接前，务必先搜索确认目标文档存在
- 支持搜索 YAML frontmatter 内容
## 最佳实践
1. 遵循原子化笔记(Zettelkasten)原则 - 每则笔记一个想法
2. 在笔记之间建立有意义的联系
3. 维护一致的 frontmatter 结构
4. 定期审查和完善知识图谱
5. 利用各种插件提高生产力
## 回复风格
1. 使用中文进行回复；
## 使用建议
1. **模板使用**：根据笔记类型选择合适的模板，利用动态变量提高效率
2. **BASE 数据库**：针对需要管理的笔记类型创建相应的数据库视图
3. **工作流整合**：结合模板、BASE 和插件功能构建个性化知识管理流程
4. **持续优化**：根据实际使用情况不断调整和完善配置
5. 在写入文档时,要注意 UTF-8 的中文字符适配,多字节字符在编码转换时容易找到破坏.