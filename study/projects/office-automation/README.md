# 项目4：自动化办公助手

**所属阶段**：第三阶段（第4-5周）  
**状态**：⏸️ 未开始  
**难度**：⭐⭐⭐⭐⭐

---

## 📋 项目目标

构建一个完整的办公自动化工作流系统，能够处理Excel、Word、PDF、PowerPoint等多种格式的文档。

---

## 🎯 学习目标

通过本项目，你将：
- ✅ 精通Excel数据处理和自动化
- ✅ 掌握Word文档生成和编辑
- ✅ 熟练使用PDF处理工具链
- ✅ 能够自动化创建PowerPoint演示文稿
- ✅ 构建完整的文档处理流水线

---

## 📦 涉及的Skills

1. **xlsx** - Excel表格操作
2. **docx** - Word文档操作
3. **pdf** - PDF文档操作
4. **pptx** - PowerPoint演示文稿操作

---

## ✅ 实施步骤

### Part 1: Excel财务分析仪表板

**目标**：创建一个自动化的财务分析系统

- [ ] 设置数据源（CSV/Excel文件）
- [ ] 使用pandas读取和处理数据
- [ ] 创建数据透视表
- [ ] 生成图表（柱状图、折线图、饼图）
- [ ] 应用格式化（颜色、字体、边框）
- [ ] 实现公式计算
- [ ] 使用recalc.py确保公式重算

**输出**：`excel/financial_dashboard.xlsx`

### Part 2: Word报告生成器

**目标**：从Excel数据自动生成格式化的Word报告

- [ ] 学习OOXML格式
- [ ] 创建报告模板
- [ ] 从Excel提取数据
- [ ] 使用docx-js生成文档
- [ ] 插入图表和表格
- [ ] 应用样式和格式
- [ ] 实现红线修订追踪

**输出**：`word/financial_report.docx`

### Part 3: PDF批量处理工具

**目标**：实现PDF的合并、提取、加密等功能

- [ ] 使用pypdf合并多个PDF
- [ ] 使用pdfplumber提取文本和表格
- [ ] 填写PDF表单
- [ ] 添加水印
- [ ] 实现PDF加密
- [ ] 批量处理功能

**输出**：`pdf/processed_reports.pdf`

### Part 4: PowerPoint自动生成

**目标**：从数据自动生成演示文稿

- [ ] 设计PPT模板
- [ ] 学习html2pptx工作流
- [ ] 从Excel/Word提取数据
- [ ] 生成幻灯片
- [ ] 批量替换文本和图像
- [ ] 生成目录和缩略图
- [ ] 使用rearrange.py重排幻灯片

**输出**：`pptx/presentation.pptx`

---

## 📂 项目结构

```
office-automation/
├── README.md                # 本文件
├── requirements.txt         # Python依赖
├── config.py               # 配置文件
├── data/                   # 测试数据
│   ├── sales_data.csv
│   ├── financial_data.xlsx
│   └── template.docx
├── excel/                  # Excel模块
│   ├── data_processor.py
│   ├── chart_generator.py
│   ├── financial_dashboard.xlsx  # 输出
│   └── recalc.py
├── word/                   # Word模块
│   ├── report_generator.py
│   ├── template_manager.py
│   ├── financial_report.docx     # 输出
│   └── scripts/
├── pdf/                    # PDF模块
│   ├── merger.py
│   ├── extractor.py
│   ├── form_filler.py
│   ├── watermark.py
│   ├── processed_reports.pdf     # 输出
│   └── scripts/
├── pptx/                   # PowerPoint模块
│   ├── slide_generator.py
│   ├── html2pptx_wrapper.js
│   ├── presentation.pptx         # 输出
│   └── scripts/
├── output/                 # 所有输出文件
├── docs/                   # 技术文档
│   ├── architecture.md
│   ├── user_manual.md
│   └── api_reference.md
└── tests/                  # 测试文件
```

---

## 📝 交付物清单

- [ ] **完整的自动化脚本（Python）** - 可运行的处理流程
- [ ] **财务分析仪表板（Excel）** - 包含图表和数据透视表
- [ ] **自动生成的报告（Word + PDF）** - 格式化的商业报告
- [ ] **数据演示文稿（PowerPoint）** - 自动生成的幻灯片
- [ ] **技术文档和使用手册** - 完整的文档说明

---

## 🔧 技术栈

### Python依赖
```txt
pandas>=2.0.0
openpyxl>=3.1.0
xlsxwriter>=3.1.0
pypdf>=3.0.0
pdfplumber>=0.10.0
reportlab>=4.0.0
defusedxml>=0.7.0
```

### Node.js依赖
```txt
docx
pptxgenjs
playwright
sharp
react-icons
```

### 系统依赖
- pandoc
- libreoffice
- poppler-utils
- qpdf

---

## 💡 实施技巧

### Excel技巧
```python
# 使用pandas处理数据
import pandas as pd
import openpyxl

# 读取数据
df = pd.read_csv('data/sales_data.csv')

# 创建数据透视表
pivot = df.pivot_table(...)

# 生成图表
# ...

# 应用格式化
from openpyxl.styles import Font, Fill
```

### Word技巧
```python
# 使用OOXML操作
# 从Excel插入数据到Word
# 应用样式和格式
```

### PDF技巧
```python
# 合并PDF
from pypdf import PdfMerger
merger = PdfMerger()
# ...

# 提取文本
import pdfplumber
with pdfplumber.open('file.pdf') as pdf:
    text = pdf.pages[0].extract_text()
```

### PowerPoint技巧
```javascript
// 使用pptxgenjs
let pptx = new PptxGenJS();
// ...
```

---

## 🔗 相关资源

### Skills文档
- [xlsx](../../../anthropics-skills/skills/xlsx/SKILL.md)
- [docx](../../../anthropics-skills/skills/docx/SKILL.md)
- [pdf](../../../anthropics-skills/skills/pdf/SKILL.md)
- [pptx](../../../anthropics-skills/skills/pptx/SKILL.md)

### 参考脚本
- `anthropics-skills/skills/xlsx/recalc.py`
- `anthropics-skills/skills/docx/scripts/document.py`
- `anthropics-skills/skills/pdf/scripts/`
- `anthropics-skills/skills/pptx/scripts/html2pptx.js`

### 学习笔记
- [第4-5周学习笔记](../../learning-notes/week-4-5-documents.md)

---

## ✨ 开始项目

### 第一步：环境准备
```bash
# 安装Python依赖
pip install -r requirements.txt

# 安装Node.js依赖
npm install -g docx pptxgenjs

# 安装系统工具
# Windows: 下载安装包
# Linux: sudo apt-get install pandoc libreoffice poppler-utils
```

### 第二步：准备测试数据
- 创建示例CSV文件
- 准备Word模板
- 收集测试PDF文件

### 第三步：逐模块实现
1. 先完成Excel模块
2. 再做Word生成
3. 然后PDF处理
4. 最后PowerPoint自动化

---

**预计完成时间**：14天（第4-5周）  
**开始日期**：  
**完成日期**：

