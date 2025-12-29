# 项目4示例：自动化办公助手

这个示例展示如何用Python自动化处理Excel、Word、PDF和PowerPoint文档。

---

## 🎯 快速开始：Excel自动化

### Step 1: 环境准备

```bash
cd study/projects/office-automation

# 创建虚拟环境
python -m venv venv
venv\Scripts\activate  # Windows

# 安装依赖
pip install pandas openpyxl xlsxwriter
```

---

### Step 2: 创建示例数据

创建文件：`data/sales_data.csv`

```csv
日期,产品,数量,单价,总额
2025-01-01,笔记本电脑,5,5999,29995
2025-01-02,鼠标,20,99,1980
2025-01-03,键盘,15,299,4485
2025-01-04,显示器,8,1999,15992
2025-01-05,笔记本电脑,3,5999,17997
2025-01-06,鼠标,25,99,2475
2025-01-07,键盘,10,299,2990
```

---

### Step 3: Excel处理示例

创建文件：`excel/simple_dashboard.py`

```python
"""
简单的Excel财务仪表板生成器
"""

import pandas as pd
import openpyxl
from openpyxl.styles import Font, PatternFill, Alignment
from openpyxl.chart import BarChart, Reference
import os

def create_financial_dashboard():
    """创建财务分析仪表板"""
    
    print("📊 开始创建Excel财务仪表板...")
    
    # 1. 读取数据
    print("   读取销售数据...")
    df = pd.read_csv('../data/sales_data.csv', encoding='utf-8-sig')
    
    # 2. 数据分析
    print("   分析数据...")
    summary = df.groupby('产品').agg({
        '数量': 'sum',
        '总额': 'sum'
    }).reset_index()
    summary['平均单价'] = summary['总额'] / summary['数量']
    
    # 3. 创建Excel文件
    print("   创建Excel文件...")
    output_file = '../output/financial_dashboard.xlsx'
    os.makedirs('../output', exist_ok=True)
    
    with pd.ExcelWriter(output_file, engine='openpyxl') as writer:
        # 写入原始数据
        df.to_excel(writer, sheet_name='原始数据', index=False)
        
        # 写入汇总数据
        summary.to_excel(writer, sheet_name='产品汇总', index=False)
        
        # 获取工作簿
        workbook = writer.book
        
        # 格式化产品汇总表
        summary_sheet = workbook['产品汇总']
        
        # 设置标题样式
        header_fill = PatternFill(start_color='2C3E50', end_color='2C3E50', fill_type='solid')
        header_font = Font(color='FFFFFF', bold=True, size=12)
        
        for cell in summary_sheet[1]:
            cell.fill = header_fill
            cell.font = header_font
            cell.alignment = Alignment(horizontal='center', vertical='center')
        
        # 设置数字格式
        for row in summary_sheet.iter_rows(min_row=2, max_row=summary_sheet.max_row):
            row[1].number_format = '#,##0'  # 数量
            row[2].number_format = '¥#,##0.00'  # 总额
            row[3].number_format = '¥#,##0.00'  # 平均单价
        
        # 调整列宽
        summary_sheet.column_dimensions['A'].width = 15
        summary_sheet.column_dimensions['B'].width = 12
        summary_sheet.column_dimensions['C'].width = 15
        summary_sheet.column_dimensions['D'].width = 15
        
        # 添加图表
        chart = BarChart()
        chart.title = "产品销售额对比"
        chart.x_axis.title = "产品"
        chart.y_axis.title = "销售额（元）"
        
        data = Reference(summary_sheet, min_col=3, min_row=1, max_row=summary_sheet.max_row)
        categories = Reference(summary_sheet, min_col=1, min_row=2, max_row=summary_sheet.max_row)
        
        chart.add_data(data, titles_from_data=True)
        chart.set_categories(categories)
        
        summary_sheet.add_chart(chart, "F2")
    
    print(f"\n✅ Excel仪表板创建完成！")
    print(f"   文件位置: {output_file}")
    print(f"   包含: 原始数据表、产品汇总表、销售图表")

if __name__ == "__main__":
    create_financial_dashboard()
```

---

### Step 4: Word文档生成示例

创建文件：`word/simple_report.py`

```python
"""
简单的Word报告生成器
"""

from docx import Document
from docx.shared import Inches, Pt, RGBColor
from docx.enum.text import WD_PARAGRAPH_ALIGNMENT
import pandas as pd
import os

def create_sales_report():
    """创建销售报告"""
    
    print("📝 开始创建Word报告...")
    
    # 读取数据
    df = pd.read_csv('../data/sales_data.csv', encoding='utf-8-sig')
    total_revenue = df['总额'].sum()
    total_quantity = df['数量'].sum()
    
    # 创建文档
    doc = Document()
    
    # 设置文档样式
    style = doc.styles['Normal']
    font = style.font
    font.name = 'Microsoft YaHei'
    font.size = Pt(11)
    
    # 添加标题
    title = doc.add_heading('销售数据分析报告', 0)
    title.alignment = WD_PARAGRAPH_ALIGNMENT.CENTER
    title_run = title.runs[0]
    title_run.font.color.rgb = RGBColor(44, 62, 80)
    
    # 添加日期
    date_para = doc.add_paragraph('报告日期：2025年1月1日 - 1月7日')
    date_para.alignment = WD_PARAGRAPH_ALIGNMENT.CENTER
    
    # 添加分隔线
    doc.add_paragraph('_' * 50)
    
    # 执行摘要
    doc.add_heading('一、执行摘要', 1)
    doc.add_paragraph(f'本周总销售额：¥{total_revenue:,.2f}')
    doc.add_paragraph(f'本周总销售量：{total_quantity} 件')
    doc.add_paragraph(f'平均客单价：¥{total_revenue/len(df):,.2f}')
    
    # 详细数据
    doc.add_heading('二、详细数据', 1)
    
    # 添加表格
    table = doc.add_table(rows=1, cols=5)
    table.style = 'Light Grid Accent 1'
    
    # 表头
    header_cells = table.rows[0].cells
    headers = ['日期', '产品', '数量', '单价', '总额']
    for i, header in enumerate(headers):
        header_cells[i].text = header
        header_cells[i].paragraphs[0].runs[0].font.bold = True
    
    # 数据行
    for _, row in df.iterrows():
        cells = table.add_row().cells
        cells[0].text = str(row['日期'])
        cells[1].text = str(row['产品'])
        cells[2].text = str(row['数量'])
        cells[3].text = f"¥{row['单价']:,.2f}"
        cells[4].text = f"¥{row['总额']:,.2f}"
    
    # 产品分析
    doc.add_heading('三、产品分析', 1)
    product_summary = df.groupby('产品')['总额'].sum().sort_values(ascending=False)
    
    for product, revenue in product_summary.items():
        doc.add_paragraph(
            f'• {product}：¥{revenue:,.2f} ({revenue/total_revenue*100:.1f}%)',
            style='List Bullet'
        )
    
    # 结论
    doc.add_heading('四、结论与建议', 1)
    top_product = product_summary.index[0]
    doc.add_paragraph(f'1. {top_product}是本周销售冠军，建议加大库存')
    doc.add_paragraph('2. 周末销量较高，可以增加促销活动')
    doc.add_paragraph('3. 建议开发组合套餐提高客单价')
    
    # 保存文档
    output_file = '../output/sales_report.docx'
    os.makedirs('../output', exist_ok=True)
    doc.save(output_file)
    
    print(f"\n✅ Word报告创建完成！")
    print(f"   文件位置: {output_file}")

if __name__ == "__main__":
    # 注意：需要先安装 python-docx
    # pip install python-docx
    create_sales_report()
```

---

### Step 5: PDF处理示例

创建文件：`pdf/simple_merge.py`

```python
"""
简单的PDF合并工具
"""

from pypdf import PdfMerger, PdfReader
import os

def merge_pdfs():
    """合并多个PDF文件"""
    
    print("📄 开始合并PDF文件...")
    
    # 创建合并器
    merger = PdfMerger()
    
    # 假设我们要合并output目录下的PDF文件
    # 实际使用时替换为真实的PDF文件
    pdf_files = [
        # '../output/report1.pdf',
        # '../output/report2.pdf',
        # '../output/report3.pdf',
    ]
    
    # 检查文件
    if not pdf_files:
        print("   提示：请将要合并的PDF文件路径添加到pdf_files列表中")
        print("   示例代码展示了合并的方法")
        
        # 创建示例说明文档
        create_sample_pdf()
        return
    
    # 合并PDF
    for pdf_file in pdf_files:
        if os.path.exists(pdf_file):
            print(f"   添加: {pdf_file}")
            merger.append(pdf_file)
    
    # 保存合并后的文件
    output_file = '../output/merged_report.pdf'
    os.makedirs('../output', exist_ok=True)
    merger.write(output_file)
    merger.close()
    
    print(f"\n✅ PDF合并完成！")
    print(f"   文件位置: {output_file}")

def create_sample_pdf():
    """创建示例PDF说明文档"""
    from reportlab.pdfgen import canvas
    from reportlab.lib.pagesizes import A4
    
    output_file = '../output/pdf_merge_guide.pdf'
    os.makedirs('../output', exist_ok=True)
    
    c = canvas.Canvas(output_file, pagesize=A4)
    c.setFont("Helvetica", 16)
    c.drawString(100, 800, "PDF Merge Tool - Usage Guide")
    c.setFont("Helvetica", 12)
    c.drawString(100, 750, "This is a sample PDF file.")
    c.drawString(100, 730, "To merge PDFs:")
    c.drawString(120, 710, "1. Add PDF file paths to pdf_files list")
    c.drawString(120, 690, "2. Run the script")
    c.drawString(120, 670, "3. Find merged PDF in output folder")
    c.save()
    
    print(f"\n   创建了示例PDF: {output_file}")

if __name__ == "__main__":
    # 注意：需要先安装 pypdf
    # pip install pypdf reportlab
    merge_pdfs()
```

---

### Step 6: 完整的自动化脚本

创建文件：`run_all.py`

```python
"""
运行所有文档处理任务
"""

import sys
import os

# 添加各模块到路径
sys.path.append('excel')
sys.path.append('word')
sys.path.append('pdf')

def main():
    """运行所有自动化任务"""
    
    print("="*60)
    print("📋 办公自动化助手")
    print("="*60)
    print()
    
    # 1. Excel处理
    print("[1/3] 处理Excel...")
    try:
        from excel.simple_dashboard import create_financial_dashboard
        create_financial_dashboard()
    except Exception as e:
        print(f"   ❌ Excel处理失败: {e}")
    print()
    
    # 2. Word生成
    print("[2/3] 生成Word报告...")
    try:
        from word.simple_report import create_sales_report
        create_sales_report()
    except Exception as e:
        print(f"   ❌ Word生成失败: {e}")
    print()
    
    # 3. PDF处理
    print("[3/3] 处理PDF...")
    try:
        from pdf.simple_merge import merge_pdfs
        merge_pdfs()
    except Exception as e:
        print(f"   ❌ PDF处理失败: {e}")
    print()
    
    print("="*60)
    print("✅ 所有任务完成！")
    print("="*60)
    print("\n📁 输出文件位置: output/")
    print("   - financial_dashboard.xlsx")
    print("   - sales_report.docx")
    print("   - pdf_merge_guide.pdf")

if __name__ == "__main__":
    main()
```

---

## 🚀 运行示例

### 方法1：单独运行

```bash
# Excel
cd excel
python simple_dashboard.py

# Word
cd word
python simple_report.py

# PDF
cd pdf
python simple_merge.py
```

### 方法2：一键运行

```bash
python run_all.py
```

---

## 📂 完整项目结构

```
office-automation/
├── README.md
├── SAMPLE.md (本文件)
├── data/
│   └── sales_data.csv ✅
├── excel/
│   └── simple_dashboard.py ✅
├── word/
│   └── simple_report.py ✅
├── pdf/
│   └── simple_merge.py ✅
├── run_all.py ✅
└── output/
    ├── financial_dashboard.xlsx
    ├── sales_report.docx
    └── pdf_merge_guide.pdf
```

---

## ✅ 检查清单

- [ ] 成功创建了虚拟环境
- [ ] 安装了所有依赖
- [ ] 创建了示例数据文件
- [ ] 成功生成了Excel仪表板
- [ ] 成功生成了Word报告
- [ ] 理解了PDF处理方法
- [ ] 运行了一键自动化脚本

---

## 💡 扩展练习

### 在Cursor中让Claude帮你

```markdown
选中代码，按 Ctrl+K，输入：

"请帮我改进这个自动化脚本：
1. 添加更多数据分析（趋势、对比）
2. 美化Excel图表样式
3. 在Word中添加更多可视化
4. 实现PPT自动生成功能
5. 添加错误处理和日志"
```

---

**开始自动化你的办公工作吧！📊**

