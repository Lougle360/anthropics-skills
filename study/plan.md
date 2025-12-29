# Skills 深度学习与实战计划

## 📚 学习路线图

本计划分为**4个阶段**，每个阶段包含**理论学习 + 实战项目**，共计**6-8周**完成全部16个skills的深度学习。

---

## 🎯 第一阶段：基础技能（第1-2周）

### 学习目标

掌握无需外部依赖的6个基础skills，建立对Skills系统的整体认知。

### 涵盖Skills

- [`brand-guidelines`](../anthropics-skills/skills/brand-guidelines/SKILL.md) - 品牌指南应用
- [`theme-factory`](../anthropics-skills/skills/theme-factory/SKILL.md) - 主题工厂
- [`skill-creator`](../anthropics-skills/skills/skill-creator/SKILL.md) - 技能创建器
- [`doc-coauthoring`](../anthropics-skills/skills/doc-coauthoring/SKILL.md) - 文档协作
- [`internal-comms`](../anthropics-skills/skills/internal-comms/SKILL.md) - 内部沟通
- [`frontend-design`](../anthropics-skills/skills/frontend-design/SKILL.md) - 前端界面设计

### 学习方法（通过Cursor）

**Day 1-2: 理解Skills结构**

```bash
# 在Cursor中使用AI助手
1. 打开 skills-intro.md，让Claude解释16个skills的分类和应用场景
2. 阅读 anthropics-skills/spec/agent-skills-spec.md 了解技能规范
3. 打开 template/SKILL.md 学习技能的基本结构（YAML frontmatter + Markdown）
```

**Day 3-4: 深入学习brand-guidelines和theme-factory**

```bash
# 在Cursor中交互式学习
1. 打开 brand-guidelines/SKILL.md，让Claude创建一个符合Anthropic品牌的示例文档
2. 打开 theme-factory/SKILL.md，让Claude展示每个预设主题的效果
3. 对比10个主题的配色和字体方案，记录到笔记中
```

**Day 5-7: 实战skill-creator**

```bash
# 动手创建你的第一个自定义skill
1. 使用 skill-creator/scripts/init_skill.py 初始化一个新技能
2. 让Claude帮你设计一个"个人简历生成器"技能
3. 使用 quick_validate.py 验证技能文件
4. 用 package_skill.py 打包技能
```

### 🎯 实战项目1：个人品牌设计系统

**项目描述**：创建一个包含品牌指南、配色主题和文档模板的个人品牌系统。

**实施步骤**：

1. 使用 `brand-guidelines` 设计你的个人品牌色彩体系
2. 在 `theme-factory` 中选择或自定义一个主题
3. 使用 `skill-creator` 创建"个人简历生成器"技能
4. 用 `doc-coauthoring` 工作流撰写一份技术博客
5. 用 `frontend-design` 原则设计一个个人主页原型

**交付物**：

- 个人品牌指南文档（PDF）
- 自定义theme配置文件
- 一个可用的自定义skill（简历生成器）
- 一篇完整的技术博客
- 个人主页原型图

---

## 🎨 第二阶段：设计与创意（第3周）

### 学习目标

掌握创意和视觉设计相关skills，学习算法艺术和画布设计。

### 涵盖Skills

- [`algorithmic-art`](../anthropics-skills/skills/algorithmic-art/SKILL.md) - 算法艺术生成
- [`canvas-design`](../anthropics-skills/skills/canvas-design/SKILL.md) - 画布设计创作

### 学习方法

**Day 8-10: 深入algorithmic-art**

```bash
# 在Cursor中学习p5.js和生成式艺术
1. 打开 algorithmic-art/SKILL.md，研究生成式艺术的设计哲学
2. 阅读 templates/generator_template.js，理解基于种子的随机生成
3. 让Claude生成3-5个不同风格的算法艺术（几何、有机、分形等）
4. 在viewer.html中预览和调整参数
```

**Day 11-14: 实战canvas-design**

```bash
# 学习专业级视觉设计
1. 研究 canvas-design/canvas-fonts 中的70+字体
2. 让Claude使用不同字体创建海报、信息图、社交媒体图像
3. 学习"视觉表达优先，文本最少"的设计理念
```

### 🎯 实战项目2：数据可视化艺术海报

**项目描述**：为一组数据（如天气、股票、音乐等）创建算法艺术作品和专业海报。

**实施步骤**：

1. 选择一个数据集（GitHub commits、天气数据、音乐播放记录等）
2. 用 `algorithmic-art` 创建3个不同风格的数据可视化艺术
3. 用 `canvas-design` 制作专业海报，应用你在阶段一学到的主题
4. 结合 `brand-guidelines` 确保品牌一致性

**交付物**：

- 3个算法艺术HTML文件
- 2-3张高质量海报（PNG/PDF）
- 设计说明文档

### 🎯 实战项目3：Slack团队表情包

**项目描述**：为团队或社区创建一套Slack动画GIF表情包。

**涉及Skills**: `slack-gif-creator`

**实施步骤**：

1. 阅读 [`slack-gif-creator/SKILL.md`](../anthropics-skills/skills/slack-gif-creator/SKILL.md)
2. 研究 `core/gif_builder.py`、`easing.py`、`frame_composer.py`
3. 设计5-10个表情动画（点赞、庆祝、思考、加载等）
4. 优化GIF大小（颜色数量、帧率、尺寸）

**交付物**：

- 10个优化后的GIF文件
- 使用说明和创意设计文档

---

## 📄 第三阶段：文档处理大师（第4-5周）

### 学习目标

深度掌握办公文档处理的4个核心skills，能够自动化处理复杂文档任务。

### 涵盖Skills

- [`xlsx`](../anthropics-skills/skills/xlsx/SKILL.md) - Excel表格操作
- [`docx`](../anthropics-skills/skills/docx/SKILL.md) - Word文档操作
- [`pdf`](../anthropics-skills/skills/pdf/SKILL.md) - PDF文档操作
- [`pptx`](../anthropics-skills/skills/pptx/SKILL.md) - PowerPoint演示文稿操作

### 学习方法

**Day 15-17: Excel自动化（xlsx）**

```bash
# 掌握pandas和openpyxl
1. 学习 xlsx/SKILL.md 中的公式、格式化、数据分析功能
2. 研究 recalc.py 脚本的工作原理
3. 让Claude创建财务报表、数据透视表、图表
4. 练习批量数据处理和自动化报告
```

**Day 18-20: Word文档操作（docx）**

```bash
# 深入OOXML和docx-js
1. 学习 docx/ooxml.md 了解Office XML格式
2. 使用 scripts/document.py 进行文档操作
3. 实践"红线工作流"（跟踪修订）
4. 学习 docx-js.md 创建新文档
```

**Day 21-23: PDF处理（pdf）**

```bash
# 掌握PDF工具链
1. 学习 pdf/SKILL.md 中的pypdf、pdfplumber、reportlab
2. 研究 scripts/ 目录中的8个脚本
3. 实践PDF表单填写（forms.md）
4. 练习文本提取、合并、分割、加密
```

**Day 24-28: PowerPoint自动化（pptx）**

```bash
# 掌握html2pptx和OOXML编辑
1. 学习 pptx/html2pptx.md，从HTML创建演示文稿
2. 研究 scripts/html2pptx.js 的工作流程
3. 使用 rearrange.py、replace.py 批量处理幻灯片
4. 用 thumbnail.py 生成缩略图
```

### 🎯 实战项目4：自动化办公助手

**项目描述**：构建一个完整的办公自动化工作流，处理多格式文档。

**实施步骤**：

1. **Excel部分**：创建一个财务分析仪表板
   - 读取多个CSV/Excel文件
   - 使用pandas进行数据分析
   - 生成图表和数据透视表
   - 应用格式化和公式

2. **Word部分**：生成标准化报告
   - 从Excel数据生成Word报告
   - 应用模板和样式
   - 插入图表和表格
   - 实现红线修订追踪

3. **PDF部分**：PDF批量处理
   - 合并多个PDF报告
   - 提取关键信息
   - 填写PDF表单
   - 添加水印和加密

4. **PowerPoint部分**：自动生成演示文稿
   - 从Word/Excel数据生成幻灯片
   - 使用html2pptx创建复杂布局
   - 批量替换文本和图像
   - 生成目录和缩略图

**交付物**：

- 完整的自动化脚本（Python）
- 财务分析仪表板（Excel）
- 自动生成的报告（Word + PDF）
- 数据演示文稿（PowerPoint）
- 技术文档和使用手册

---

## 🌐 第四阶段：Web开发与工具（第6-7周）

### 学习目标

掌握Web开发、测试和MCP服务器构建等高级技能。

### 涵盖Skills

- [`web-artifacts-builder`](../anthropics-skills/skills/web-artifacts-builder/SKILL.md) - Web构件构建器
- [`webapp-testing`](../anthropics-skills/skills/webapp-testing/SKILL.md) - Web应用测试
- [`mcp-builder`](../anthropics-skills/skills/mcp-builder/SKILL.md) - MCP服务器构建

### 学习方法

**Day 29-35: Web全栈开发**

```bash
# 学习React + TypeScript + Vite
1. 研究 web-artifacts-builder/SKILL.md
2. 使用 scripts/init-artifact.sh 初始化项目
3. 学习shadcn/ui组件库的使用
4. 用 bundle-artifact.sh 打包部署

# 学习Playwright自动化测试
5. 研究 webapp-testing/SKILL.md
6. 学习 scripts/with_server.py 的服务器管理
7. 研究 examples/ 中的3个测试示例
8. 实践"侦察-行动"测试模式
```

**Day 36-42: MCP服务器开发**

```bash
# 深入MCP协议
1. 学习 mcp-builder/reference/ 中的4个参考文档
2. 研究 scripts/connections.py 和 evaluation.py
3. 对比TypeScript和Python实现方式
4. 学习MCP最佳实践和设计模式
```

### 🎯 实战项目5：数据可视化仪表板

**项目描述**：创建一个完整的数据可视化Web应用，集成前端、后端和测试。

**实施步骤**：

1. 使用 `web-artifacts-builder` 创建React+TypeScript应用
2. 集成 shadcn/ui 组件库
3. 使用阶段三的Excel数据作为数据源
4. 实现交互式图表和筛选功能
5. 用 `webapp-testing` 编写自动化测试
6. 应用 `frontend-design` 原则优化UI

**交付物**：

- 完整的Web应用（React + TypeScript）
- Playwright测试套件
- 部署包和文档

### 🎯 实战项目6：自定义MCP服务器

**项目描述**：构建一个MCP服务器，扩展Claude的文档处理能力。

**实施步骤**：

1. 设计MCP服务器功能（如"文档转换服务"）
2. 选择Python或TypeScript实现
3. 实现工具、资源和提示功能
4. 使用 `evaluation.py` 进行质量评估
5. 编写完整的文档和测试

**交付物**：

- MCP服务器源代码（TypeScript/Python）
- 配置文件和文档
- 测试用例和评估报告

---

## 🎓 第五阶段：综合实战（第8周）

### 🎯 实战项目7：企业自动化系统（毕业项目）

**项目描述**：综合运用所有16个skills，构建一个完整的企业自动化系统。

**系统模块**：

1. **品牌与设计模块**
   - 使用 `brand-guidelines` + `theme-factory` 建立企业VI系统
   - 用 `canvas-design` 创建营销素材
   - 用 `algorithmic-art` 生成品牌视觉元素

2. **文档处理模块**
   - 用 `xlsx` 处理财务和数据报表
   - 用 `docx` 生成合同和报告
   - 用 `pdf` 处理归档和表单
   - 用 `pptx` 自动生成演示文稿

3. **Web应用模块**
   - 用 `web-artifacts-builder` 构建管理后台
   - 用 `webapp-testing` 确保质量
   - 用 `frontend-design` 优化界面

4. **工具扩展模块**
   - 用 `mcp-builder` 创建自定义工具
   - 用 `skill-creator` 创建企业专属技能

5. **协作沟通模块**
   - 用 `doc-coauthoring` 管理文档协作
   - 用 `internal-comms` 标准化内部沟通
   - 用 `slack-gif-creator` 丰富团队文化

**交付物**：

- 完整的企业自动化系统
- 所有模块的源代码和文档
- 技术架构文档
- 用户手册和培训材料
- 演示视频

---

## 🛠️ 在Cursor中的学习最佳实践

### 1. **交互式文档学习**

```bash
# 打开任何SKILL.md文件，然后：
1. 选中一段内容，按 Cmd+K (Mac) 或 Ctrl+K (Windows)
2. 询问Claude："请解释这段内容的工作原理"
3. 要求Claude给出实际示例："用这个技能创建一个例子"
```

### 2. **代码生成与验证**

```bash
# 在Cursor中：
1. 打开相关的scripts目录
2. 让Claude解释脚本功能
3. 要求生成新的脚本或修改现有脚本
4. 使用Cursor的内置终端运行和测试
```

### 3. **项目驱动学习**

```bash
# 每个实战项目：
1. 在Cursor中创建项目文件夹
2. 让Claude帮你搭建项目结构
3. 逐步实现功能，边做边学
4. 使用Git版本控制记录进度
```

### 4. **对比学习**

```bash
# 学习相似skills时：
1. 并排打开2-3个SKILL.md文件
2. 让Claude对比它们的异同
3. 理解不同工具的适用场景
```

### 5. **创建学习笔记**

```bash
# 在项目根目录创建：
learning-notes/
  ├── week-1-basics.md
  ├── week-2-design.md
  ├── week-3-documents.md
  └── ...

# 每天记录：
- 学到的关键概念
- 遇到的问题和解决方案
- 有用的代码片段
- 项目进度
```

---

## 📊 学习进度追踪

### 每周检查清单

**第1-2周**：

- [ ] 理解16个skills的分类和用途
- [ ] 掌握SKILL.md的结构
- [ ] 完成品牌设计系统项目
- [ ] 创建第一个自定义skill

**第3周**：

- [ ] 掌握algorithmic-art和canvas-design
- [ ] 完成数据可视化艺术项目
- [ ] 完成Slack表情包项目

**第4-5周**：

- [ ] 精通Excel/Word/PDF/PowerPoint操作
- [ ] 完成自动化办公助手项目
- [ ] 能够处理复杂文档任务

**第6-7周**：

- [ ] 掌握React+TypeScript开发
- [ ] 学会Playwright测试
- [ ] 完成数据可视化仪表板
- [ ] 构建第一个MCP服务器

**第8周**：

- [ ] 完成综合实战项目
- [ ] 整理所有项目代码
- [ ] 撰写技术文档
- [ ] 制作演示视频

---

## 💡 学习资源

### 必读文档

- [`skills-intro.md`](../skills-intro.md) - 技能介绍
- [`skills-dependencies.md`](../skills-dependencies.md) - 依赖清单
- [`anthropics-skills/README.md`](../anthropics-skills/README.md) - 项目总览
- [`anthropics-skills/spec/agent-skills-spec.md`](../anthropics-skills/spec/agent-skills-spec.md) - 技能规范

### 参考链接

- [What are Skills?](https://support.claude.com/en/articles/12512176-what-are-skills)
- [Using Skills in Claude](https://support.claude.com/en/articles/12512180-using-skills-in-claude)
- [Creating Custom Skills](https://support.claude.com/en/articles/12512198-creating-custom-skills)
- [Skills API Quickstart](https://docs.claude.com/en/api/skills-guide#creating-a-skill)

---

## 🎯 学习成果

完成本计划后，你将能够：

✅ **理解并运用所有16个Anthropic Skills**  
✅ **独立创建自定义Skills**  
✅ **构建自动化办公工作流**  
✅ **开发完整的Web应用**  
✅ **创建MCP服务器扩展Claude功能**  
✅ **完成6-7个实战项目**  
✅ **建立个人技能库和工具集**

---

## 📅 时间分配建议

- **每天学习时间**：2-3小时
- **理论学习**：30%
- **动手实践**：50%
- **项目开发**：20%

**总计**：约 **100-120小时** 的深度学习

---

**祝学习愉快！遇到问题随时在Cursor中向Claude提问。**

