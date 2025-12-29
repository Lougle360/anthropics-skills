# 项目1示例：个人品牌设计系统

这是一个完整的示例，展示如何一步步完成项目1。

---

## 📝 Step-by-Step 操作指南

### Step 1: 设计个人品牌色彩体系

#### 1.1 在Cursor中操作

```markdown
1. 打开 Cursor
2. 按 Ctrl+K (或 Cmd+K)
3. 输入提示词：

"请帮我设计一个个人品牌色彩体系，包括：
- 主色：一个代表我职业形象的颜色
- 辅色：2-3个配合的辅助颜色
- 背景色：浅色和深色各一个
- 强调色：用于突出重要信息

我是一名软件工程师，希望品牌色彩体现：
- 专业、技术感
- 现代、简洁
- 可靠、稳重

请提供16进制色值和应用建议。"
```

#### 1.2 示例输出

创建文件：`brand-guide/colors.md`

```markdown
# 个人品牌色彩方案

## 主色 (Primary)
- **深蓝色** #2C3E50
- 用途：标题、重要按钮、主要元素
- 象征：专业、可靠、技术

## 辅色 (Secondary)
- **天蓝色** #3498DB
  - 用途：链接、次要按钮、装饰
- **青色** #1ABC9C
  - 用途：成功提示、亮点
- **灰蓝色** #7F8C8D
  - 用途：次要文字、分隔线

## 背景色 (Background)
- **浅色模式** #FFFFFF (白) / #ECF0F1 (浅灰)
- **深色模式** #2C3E50 (深蓝) / #34495E (墨蓝)

## 强调色 (Accent)
- **橙色** #E67E22
- 用途：Call-to-action、重要提示

## 色彩搭配示例
```

---

### Step 2: 选择或自定义主题

#### 2.1 在Cursor中操作

```markdown
1. 打开 ../../../anthropics-skills/skills/theme-factory/SKILL.md
2. 选中主题列表部分
3. 按 Ctrl+K，输入：

"这10个主题中，哪个最适合软件工程师的个人品牌？
请分析每个主题的特点，并推荐最佳选择。
然后帮我基于推荐的主题，创建一个自定义配置。"
```

#### 2.2 创建主题配置

创建文件：`theme-config.md`

```markdown
# 个人品牌主题配置

基于 **Tech Innovation** 主题定制

## 字体配对

### 标题字体
- **Primary**: Poppins (Bold, SemiBold)
- **用于**: H1, H2, Logo

### 正文字体  
- **Body**: Inter (Regular, Medium)
- **用于**: 段落文字、说明

### 代码字体
- **Monospace**: JetBrains Mono
- **用于**: 代码片段、技术内容

## 应用场景

### 简历
- 标题：Poppins Bold, 24pt, #2C3E50
- 正文：Inter Regular, 11pt, #34495E
- 强调：Inter Medium, #E67E22

### 博客
- 文章标题：Poppins SemiBold, 32pt
- 段落：Inter Regular, 16pt, line-height: 1.6
- 代码：JetBrains Mono, 14pt
```

---

### Step 3: 创建简历生成器技能

#### 3.1 初始化技能

创建目录：`resume-generator/`

创建文件：`resume-generator/SKILL.md`

```markdown
---
name: resume-generator
description: 根据个人信息自动生成格式化的专业简历，应用统一的品牌指南和主题样式
---

# Resume Generator - 简历生成器

这个技能帮助快速生成专业、一致的个人简历。

## 使用方法

提供以下信息：
1. 个人基本信息（姓名、联系方式、简介）
2. 工作经历
3. 教育背景
4. 技能列表
5. 项目经历

## 输出格式

生成的简历将包含：
- 统一的品牌色彩（#2C3E50 主色）
- 标准字体（Poppins + Inter）
- 清晰的信息层次
- 专业的排版

## 示例

### 输入
\`\`\`json
{
  "name": "张三",
  "title": "全栈工程师",
  "email": "zhangsan@example.com",
  "phone": "+86 138-0000-0000",
  "summary": "5年全栈开发经验，擅长React和Node.js...",
  "experience": [...],
  "education": [...],
  "skills": ["React", "TypeScript", "Node.js", ...]
}
\`\`\`

### 输出
生成 Markdown 格式的简历，可轻松转换为 PDF。

## 品牌一致性

所有生成的简历自动应用：
- 个人品牌色彩方案
- 统一的字体系统
- 标准的间距和布局
```

#### 3.2 创建简历模板

创建文件：`resume-generator/templates/resume-template.md`

```markdown
# {NAME}
## {TITLE}

📧 {EMAIL} | 📱 {PHONE} | 🌐 {WEBSITE}

---

## 💼 专业简介

{SUMMARY}

---

## 💻 工作经历

### {COMPANY} | {POSITION}
*{START_DATE} - {END_DATE}*

- {ACHIEVEMENT_1}
- {ACHIEVEMENT_2}
- {ACHIEVEMENT_3}

---

## 🎓 教育背景

### {UNIVERSITY} | {DEGREE}
*{GRADUATION_YEAR}*

{MAJOR}

---

## 🛠️ 技术技能

**编程语言**: {LANGUAGES}
**框架工具**: {FRAMEWORKS}
**数据库**: {DATABASES}
**其他**: {OTHERS}

---

## 🚀 项目经历

### {PROJECT_NAME}
*{PROJECT_DATE}*

{PROJECT_DESCRIPTION}

**技术栈**: {TECH_STACK}
**成果**: {RESULTS}
```

---

### Step 4: 撰写技术博客

#### 4.1 使用doc-coauthoring工作流

在Cursor中创建：`blog-post.md`

```markdown
# 我的Skills学习之旅：第一周总结

*发布日期：2025-12-29*

---

## 📝 引言

这是我学习Anthropic Skills的第一周总结。在这一周中，我掌握了6个基础技能，并完成了个人品牌设计系统项目。

## 🎯 本周学习内容

### 1. 品牌指南 (brand-guidelines)
学会了如何建立一致的品牌形象：
- 色彩系统设计
- 字体配对原则
- 品牌应用规范

### 2. 主题工厂 (theme-factory)
探索了10个预设主题，并基于Tech Innovation主题创建了自定义配置。

### 3. 技能创建器 (skill-creator)
最大的收获是创建了第一个自定义技能 - Resume Generator！

## 💡 关键收获

1. **系统化思维**：品牌设计需要系统性考虑
2. **工具思维**：Skills让Claude变成了专业工具
3. **实践为主**：动手做比只看文档效果好10倍

## 🚀 下周计划

- 学习算法艺术生成
- 创建数据可视化作品
- 制作Slack表情包

## 🔗 相关资源

- [我的品牌指南](./brand-guide/guidelines.pdf)
- [自定义主题配置](./theme-config.md)
- [Resume Generator技能](./resume-generator/)

---

*这篇博客使用了我的个人品牌色彩（#2C3E50）和字体（Poppins + Inter）*
```

---

### Step 5: 设计个人主页原型

#### 5.1 创建线框图描述

创建文件：`homepage-prototype/wireframe.md`

```markdown
# 个人主页线框图

## 页面结构

```
+------------------------------------------+
|            HEADER                        |
|  Logo        Nav: About | Work | Blog   |
+------------------------------------------+
|                                          |
|         HERO SECTION                     |
|                                          |
|    Hi, I'm [Name]                        |
|    Full-Stack Developer                  |
|                                          |
|    [CTA Button: View My Work]            |
|                                          |
+------------------------------------------+
|                                          |
|         ABOUT SECTION                    |
|                                          |
|  [Photo]  [Brief Bio]                    |
|                                          |
+------------------------------------------+
|                                          |
|         SKILLS SECTION                   |
|                                          |
|  [Tech Icon] [Tech Icon] [Tech Icon]     |
|                                          |
+------------------------------------------+
|                                          |
|         PROJECTS SECTION                 |
|                                          |
|  +--------+  +--------+  +--------+      |
|  |Project1|  |Project2|  |Project3|      |
|  +--------+  +--------+  +--------+      |
|                                          |
+------------------------------------------+
|                                          |
|         CONTACT SECTION                  |
|                                          |
|  Email | GitHub | LinkedIn               |
|                                          |
+------------------------------------------+
|            FOOTER                        |
|  © 2025 [Name]. All rights reserved.    |
+------------------------------------------+
```

## 设计规范

### 色彩应用
- Header背景：#2C3E50
- Hero Section：渐变 #2C3E50 → #34495E
- CTA Button：#E67E22
- Links：#3498DB

### 字体应用
- Logo：Poppins Bold, 24px
- H1：Poppins Bold, 48px
- H2：Poppins SemiBold, 32px
- Body：Inter Regular, 16px

### 间距
- Section padding：80px (上下)
- Container max-width：1200px
- Element spacing：24px
```

#### 5.2 在Cursor中生成高保真原型

```markdown
按 Ctrl+K，输入：

"基于上面的线框图和设计规范，帮我生成一个HTML+CSS的个人主页原型。

要求：
1. 使用我的品牌色彩（#2C3E50, #3498DB, #E67E22等）
2. 应用Poppins和Inter字体
3. 响应式设计
4. 现代、简洁的风格
5. 包含动画效果

请生成完整的HTML和CSS代码。"
```

---

## 📦 完整项目结构示例

完成后，你的项目应该是这样的：

```
personal-brand-system/
├── README.md
├── SAMPLE.md (本文件)
├── brand-guide/
│   ├── colors.md ✅
│   ├── typography.md ✅
│   └── guidelines.pdf (导出后)
├── theme-config.md ✅
├── resume-generator/
│   ├── SKILL.md ✅
│   ├── templates/
│   │   └── resume-template.md ✅
│   └── examples/
│       └── my-resume.md
├── blog-post.md ✅
└── homepage-prototype/
    ├── wireframe.md ✅
    ├── index.html
    └── style.css
```

---

## ✅ 检查清单

完成项目前，确认你已经：

- [ ] 创建了完整的色彩方案文档
- [ ] 选择并配置了主题
- [ ] 创建了resume-generator技能（包含SKILL.md）
- [ ] 撰写了一篇技术博客
- [ ] 设计了个人主页原型（至少有线框图）
- [ ] 所有文档都应用了统一的品牌指南
- [ ] 在Cursor中向Claude展示你的作品并获取反馈

---

## 💡 提示

1. **不要追求完美**：第一次完成60%就很好了
2. **多用Claude**：遇到卡顿随时按Ctrl+K求助
3. **保存过程**：把Claude的好建议都记录到笔记中
4. **迭代改进**：先完成基础版，再逐步优化

---

**现在开始动手吧！祝你完成第一个项目！🚀**

