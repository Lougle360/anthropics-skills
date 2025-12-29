# Skills 环境依赖清单

本文档详细列出所有需要外部依赖的技能，包括 Python 包、Node.js 包、系统工具等。

---

## 📋 **依赖概览**

### 有依赖的技能（10个）
✅ **重度依赖**：pptx, docx, pdf, xlsx, slack-gif-creator, web-artifacts-builder, webapp-testing, mcp-builder  
✅ **中度依赖**：algorithmic-art  
✅ **轻度依赖**：canvas-design

### 无依赖的技能（6个）
✅ brand-guidelines, theme-factory, frontend-design, skill-creator, doc-coauthoring, internal-comms

---

## 🔴 **重度依赖技能**

### 1. **pptx** - PowerPoint 演示文稿操作

#### Python 依赖
```bash
pip install "markitdown[pptx]"    # 文本提取
pip install defusedxml            # 安全的 XML 解析
```

#### Node.js 依赖
```bash
npm install -g pptxgenjs          # 通过 html2pptx 创建演示文稿
npm install -g playwright         # HTML 渲染
npm install -g react-icons react react-dom  # 图标支持
npm install -g sharp              # SVG 栅格化和图像处理
```

#### 系统依赖
```bash
sudo apt-get install libreoffice      # PDF 转换
sudo apt-get install poppler-utils    # pdftoppm 将 PDF 转换为图像
```

---

### 2. **docx** - Word 文档操作

#### Node.js 依赖
```bash
npm install -g docx               # 创建新文档
```

#### 系统依赖
```bash
sudo apt-get install pandoc           # 文本提取
sudo apt-get install libreoffice      # PDF 转换
sudo apt-get install poppler-utils    # pdftoppm 将 PDF 转换为图像
```

#### Python 依赖
```bash
pip install defusedxml            # 安全的 XML 解析
```

---

### 3. **pdf** - PDF 文档操作

#### Python 依赖（核心）
```bash
pip install pypdf                 # 基本操作（合并、拆分、旋转）
pip install pdfplumber            # 文本和表格提取
pip install reportlab             # PDF 创建
```

#### Python 依赖（可选 - OCR）
```bash
pip install pytesseract           # OCR 文字识别
pip install pdf2image             # PDF 转图像用于 OCR
```

#### 系统依赖
```bash
sudo apt-get install poppler-utils    # pdftotext 等命令行工具
sudo apt-get install qpdf             # PDF 高级操作
sudo apt-get install tesseract-ocr    # OCR 引擎（可选）
```

---

### 4. **xlsx** - Excel 表格操作

#### Python 依赖
```bash
pip install pandas                # 数据操作
pip install openpyxl              # Excel 文件读写
pip install xlsxwriter            # Excel 文件创建（可选）
```

#### 特殊要求
- **必需**：技能包含 `recalc.py` 脚本，用于公式重新计算
- **依赖**：需要 `openpyxl` 来执行 `recalc.py`

---

### 5. **slack-gif-creator** - Slack GIF 创建器

#### Python 依赖（来自 requirements.txt）
```bash
pip install pillow>=10.0.0        # 图像处理和绘图
pip install imageio>=2.31.0       # GIF 读写
pip install imageio-ffmpeg>=0.4.9 # 视频编解码器支持
pip install numpy>=1.24.0         # 数值计算
```

#### 核心工具
- `core/gif_builder.py` - GIF 构建器
- `core/validators.py` - 验证器
- `core/easing.py` - 缓动函数
- `core/frame_composer.py` - 帧合成器

---

### 6. **web-artifacts-builder** - Web 构件构建器

#### Node.js 依赖（自动安装）
技能脚本会自动安装以下依赖：

**开发依赖（通过 init-artifact.sh）：**
```bash
# React 和 TypeScript 生态系统
npm install react react-dom
npm install -D @types/react @types/react-dom
npm install -D vite @vitejs/plugin-react
npm install -D typescript

# Tailwind CSS 和 shadcn/ui
npm install -D tailwindcss postcss autoprefixer
npm install -D @shadcn/ui
# 40+ shadcn/ui 组件及其 Radix UI 依赖项
```

**打包依赖（通过 bundle-artifact.sh）：**
```bash
npm install -D parcel
npm install -D @parcel/config-default
npm install -D parcel-resolver-tspaths
npm install -D html-inline
```

#### 系统要求
- **Node.js 18+**（脚本自动检测版本）
- **npm** 或 **yarn**

---

### 7. **webapp-testing** - Web 应用测试

#### Python 依赖
```bash
pip install playwright            # 浏览器自动化
playwright install chromium       # 安装 Chromium 浏览器
```

#### 辅助工具
- `scripts/with_server.py` - 服务器生命周期管理（支持多个服务器）

#### 系统要求
- **Chromium** 浏览器（通过 Playwright 安装）
- 无头浏览器环境支持

---

### 8. **mcp-builder** - MCP 服务器构建

#### TypeScript MCP 服务器
```bash
npm install @modelcontextprotocol/sdk
npm install -D @types/node typescript
npm install -D @modelcontextprotocol/inspector  # 测试工具
```

#### Python MCP 服务器
```bash
pip install mcp                   # MCP Python SDK
# 或安装特定依赖
pip install anthropic-mcp         # 如果使用 Anthropic MCP
```

#### 开发和测试工具
```bash
# MCP Inspector（用于测试）
npx @modelcontextprotocol/inspector

# TypeScript 构建
npm run build
```

---

## 🟡 **中度依赖技能**

### 9. **algorithmic-art** - 算法艺术生成

#### 运行时要求
- **浏览器环境**：需要支持 HTML5 Canvas 的现代浏览器
- **p5.js**：通过 CDN 加载（无需本地安装）
  ```html
  <script src="https://cdnjs.cloudflare.com/ajax/libs/p5.js/1.7.0/p5.min.js"></script>
  ```

#### 模板文件
- `templates/viewer.html` - HTML 查看器模板
- `templates/generator_template.js` - p5.js 代码模板

#### 特点
- ✅ 无需本地安装 Node.js 或 Python 包
- ✅ 直接在浏览器中运行
- ⚠️ 需要现代浏览器（支持 ES6+）

---

## 🟢 **轻度依赖技能**

### 10. **canvas-design** - 画布设计创作

#### 字体资源
- **包含 70+ 字体文件**（.ttf 格式）
- 位于 `canvas-fonts/` 目录
- 包括 OFL 开源字体许可证

#### Python 依赖（推荐）
用于创建 PNG/PDF 图像：
```bash
pip install pillow                # 图像处理
pip install reportlab             # PDF 创建（可选）
```

#### 特点
- ✅ 主要依赖预打包的字体文件
- ✅ 可以使用系统自带的图像处理工具
- ⚠️ 对于复杂设计，推荐使用 PIL/Pillow

---

## ✅ **无依赖技能**

以下技能不需要特殊环境依赖，仅需要 Claude 和标准 Markdown 支持：

### 11. **brand-guidelines** - 品牌指南应用
- 纯指南性技能
- 定义 Anthropic 品牌色彩和排版规范

### 12. **theme-factory** - 主题工厂
- 包含预定义主题配置（Markdown 文件）
- `themes/` 目录中的 10 个主题文件
- `theme-showcase.pdf` 展示文件

### 13. **frontend-design** - 前端界面设计
- 纯指南性技能
- 设计原则和最佳实践文档

### 14. **skill-creator** - 技能创建器
- 包含 Python 脚本用于技能初始化和打包
- 脚本依赖：Python 3.x（系统标配）
- `scripts/init_skill.py` 和 `scripts/package_skill.py`

### 15. **doc-coauthoring** - 文档协作
- 纯工作流指南
- 结构化文档协作流程

### 16. **internal-comms** - 内部沟通
- 纯模板和指南
- `examples/` 目录中的示例文档

---

## 📊 **依赖类型统计**

### 按依赖类型分类

| 依赖类型 | 技能数量 | 技能列表 |
|---------|---------|---------|
| **Python 包** | 6 | pptx, docx, pdf, xlsx, slack-gif-creator, webapp-testing |
| **Node.js 包** | 5 | pptx, docx, web-artifacts-builder, webapp-testing, mcp-builder |
| **系统工具** | 4 | pptx, docx, pdf, webapp-testing |
| **浏览器** | 2 | algorithmic-art, webapp-testing |
| **字体文件** | 1 | canvas-design |
| **无依赖** | 6 | brand-guidelines, theme-factory, frontend-design, skill-creator, doc-coauthoring, internal-comms |

### 按依赖程度分类

| 程度 | 数量 | 说明 |
|-----|------|------|
| **重度依赖** | 8 | 需要多个外部包和系统工具 |
| **中度依赖** | 1 | 需要浏览器环境或单一外部依赖 |
| **轻度依赖** | 1 | 主要依赖打包资源，可选外部依赖 |
| **无依赖** | 6 | 仅需要 Claude 和标准工具 |

---

## 🔧 **快速安装指南**

### 完整环境设置（安装所有依赖）

#### 1. Python 依赖
```bash
# 文档处理
pip install "markitdown[pptx]" defusedxml
pip install pypdf pdfplumber reportlab
pip install pandas openpyxl xlsxwriter

# 图像和 GIF 处理
pip install pillow>=10.0.0
pip install imageio>=2.31.0 imageio-ffmpeg>=0.4.9
pip install numpy>=1.24.0

# Web 测试
pip install playwright
playwright install chromium

# OCR（可选）
pip install pytesseract pdf2image
```

#### 2. Node.js 依赖
```bash
# 文档处理
npm install -g pptxgenjs playwright docx

# 图像和图标
npm install -g react-icons react react-dom sharp

# MCP 开发
npm install -g @modelcontextprotocol/inspector
```

#### 3. 系统工具（Linux/Ubuntu）
```bash
# 文档转换
sudo apt-get install pandoc libreoffice

# PDF 工具
sudo apt-get install poppler-utils qpdf

# OCR（可选）
sudo apt-get install tesseract-ocr
```

#### 4. 系统工具（macOS）
```bash
# 使用 Homebrew
brew install pandoc poppler qpdf tesseract

# LibreOffice
brew install --cask libreoffice
```

#### 5. 系统工具（Windows）
```powershell
# 使用 Chocolatey
choco install pandoc poppler qpdf tesseract

# 或手动下载安装：
# - LibreOffice: https://www.libreoffice.org/
# - Pandoc: https://pandoc.org/
# - Poppler: https://github.com/oschwartz10612/poppler-windows/releases/
```

---

## ⚠️ **特殊注意事项**

### 1. **MCP 技能依赖**
- **mcp-builder** 技能本身用于构建 MCP 服务器
- 需要根据目标语言（TypeScript/Python）安装相应 MCP SDK
- 不是 MCP 协议本身的依赖

### 2. **跨平台兼容性**
- **LibreOffice** 和 **Pandoc**：在 Windows 上需要手动安装
- **Playwright**：首次使用需要运行 `playwright install` 安装浏览器
- **系统路径**：确保所有命令行工具在系统 PATH 中

### 3. **可选依赖**
某些依赖仅在特定场景下需要：
- **OCR 相关**（pytesseract, tesseract-ocr）：仅处理扫描 PDF 时需要
- **xlsxwriter**：pandas 已支持 Excel，通常不需要额外安装
- **pdf2image**：仅 OCR 工作流需要

### 4. **自动管理的依赖**
以下技能会自动管理依赖：
- **web-artifacts-builder**：`init-artifact.sh` 和 `bundle-artifact.sh` 自动安装 npm 包
- **algorithmic-art**：p5.js 通过 CDN 加载，无需本地安装

---

## 📖 **相关文档**

- **主介绍文档**：`skills-intro.md`
- **各技能详细文档**：`anthropics-skills/skills/<技能名称>/SKILL.md`
- **Python 包索引**：https://pypi.org/
- **Node.js 包索引**：https://www.npmjs.com/

---

**最后更新**：2025年12月29日  
**维护状态**：所有依赖项已验证 ✅

