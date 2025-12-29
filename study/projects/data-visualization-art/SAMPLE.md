# 项目2示例：数据可视化艺术海报

这个示例展示如何用p5.js创建算法艺术，并制作专业海报。

---

## 🎯 快速开始：创建你的第一个算法艺术

### Step 1: 准备数据

创建文件：`data/sample_data.json`

```json
{
  "title": "我的一周活动数据",
  "data": [
    { "day": "周一", "steps": 8234, "mood": 7, "productivity": 8 },
    { "day": "周二", "steps": 6789, "mood": 6, "productivity": 7 },
    { "day": "周三", "steps": 9456, "mood": 8, "productivity": 9 },
    { "day": "周四", "steps": 7234, "mood": 7, "productivity": 8 },
    { "day": "周五", "steps": 10234, "mood": 9, "productivity": 6 },
    { "day": "周六", "steps": 12456, "mood": 9, "productivity": 4 },
    { "day": "周日", "steps": 5678, "mood": 8, "productivity": 5 }
  ]
}
```

---

### Step 2: 创建几何风格艺术

创建文件：`art/geometric/index.html`

```html
<!DOCTYPE html>
<html lang="zh">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>几何风格 - 一周活动数据</title>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/p5.js/1.7.0/p5.min.js"></script>
    <style>
        body {
            margin: 0;
            padding: 20px;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            background: #f5f5f5;
            font-family: 'Arial', sans-serif;
        }
        #canvas-container {
            box-shadow: 0 10px 40px rgba(0,0,0,0.1);
        }
    </style>
</head>
<body>
    <script>
        // 数据
        const weekData = [
            { day: "周一", steps: 8234, mood: 7, productivity: 8 },
            { day: "周二", steps: 6789, mood: 6, productivity: 7 },
            { day: "周三", steps: 9456, mood: 8, productivity: 9 },
            { day: "周四", steps: 7234, mood: 7, productivity: 8 },
            { day: "周五", steps: 10234, mood: 9, productivity: 6 },
            { day: "周六", steps: 12456, mood: 9, productivity: 4 },
            { day: "周日", steps: 5678, mood: 8, productivity: 5 }
        ];

        function setup() {
            createCanvas(800, 800);
            background(250);
            noLoop(); // 静态图像
            
            // 设置随机种子，确保可重现
            randomSeed(42);
            
            // 绘制标题
            fill(44, 62, 80);
            textSize(24);
            textAlign(CENTER);
            text("我的一周活动数据", width/2, 50);
            
            // 绘制数据可视化
            drawGeometricViz();
        }

        function drawGeometricViz() {
            const margin = 100;
            const cols = 7;
            const cellWidth = (width - 2 * margin) / cols;
            
            for (let i = 0; i < weekData.length; i++) {
                const data = weekData[i];
                const x = margin + i * cellWidth + cellWidth/2;
                const y = height/2;
                
                // 映射数据到视觉属性
                const size = map(data.steps, 5000, 13000, 30, 100);
                const hue = map(data.mood, 1, 10, 200, 0); // 蓝到红
                const brightness = map(data.productivity, 1, 10, 180, 255);
                
                // 绘制圆形
                push();
                translate(x, y);
                
                // 颜色：心情
                fill(hue, 100, brightness);
                stroke(44, 62, 80);
                strokeWeight(2);
                
                // 大小：步数
                circle(0, 0, size);
                
                // 添加文字
                fill(44, 62, 80);
                noStroke();
                textSize(12);
                text(data.day, 0, size/2 + 20);
                textSize(10);
                text(`${data.steps}步`, 0, size/2 + 35);
                
                pop();
            }
            
            // 添加图例
            drawLegend();
        }

        function drawLegend() {
            const x = 100;
            const y = height - 80;
            
            fill(44, 62, 80);
            textSize(12);
            textAlign(LEFT);
            text("圆形大小 = 步数", x, y);
            text("颜色 = 心情 (蓝→红：低→高)", x, y + 20);
            text("亮度 = 生产力", x, y + 40);
        }

        // 保存图片
        function keyPressed() {
            if (key === 's' || key === 'S') {
                saveCanvas('geometric-art', 'png');
            }
        }
    </script>
</body>
</html>
```

---

### Step 3: 创建有机风格艺术

创建文件：`art/organic/index.html`

```html
<!DOCTYPE html>
<html lang="zh">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>有机风格 - 一周活动数据</title>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/p5.js/1.7.0/p5.min.js"></script>
    <style>
        body {
            margin: 0;
            padding: 20px;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            background: #2c3e50;
        }
    </style>
</head>
<body>
    <script>
        const weekData = [
            { day: "周一", steps: 8234, mood: 7, productivity: 8 },
            { day: "周二", steps: 6789, mood: 6, productivity: 7 },
            { day: "周三", steps: 9456, mood: 8, productivity: 9 },
            { day: "周四", steps: 7234, mood: 7, productivity: 8 },
            { day: "周五", steps: 10234, mood: 9, productivity: 6 },
            { day: "周六", steps: 12456, mood: 9, productivity: 4 },
            { day: "周日", steps: 5678, mood: 8, productivity: 5 }
        ];

        function setup() {
            createCanvas(800, 800);
            background(44, 62, 80);
            noLoop();
            
            drawOrganicFlow();
        }

        function drawOrganicFlow() {
            // 使用噪声生成流动的形态
            noiseSeed(42);
            
            for (let i = 0; i < weekData.length; i++) {
                const data = weekData[i];
                
                // 起始位置
                const startX = map(i, 0, weekData.length-1, 100, width-100);
                const startY = height/2;
                
                // 参数
                const points = 50;
                const flowLength = map(data.steps, 5000, 13000, 100, 300);
                const thickness = map(data.productivity, 1, 10, 1, 5);
                const hue = map(data.mood, 1, 10, 200, 350);
                
                // 绘制流线
                noFill();
                strokeWeight(thickness);
                
                beginShape();
                for (let j = 0; j <= points; j++) {
                    const t = j / points;
                    const x = startX;
                    const y = startY - flowLength * t;
                    
                    // 添加噪声产生有机感
                    const noiseVal = noise(i * 0.5, j * 0.1) * 50 - 25;
                    const xOffset = noiseVal;
                    
                    // 颜色渐变
                    const alpha = map(t, 0, 1, 255, 50);
                    stroke(hue % 360, 80, 90, alpha);
                    
                    vertex(x + xOffset, y);
                }
                endShape();
                
                // 标签
                fill(255, 200);
                noStroke();
                textSize(10);
                textAlign(CENTER);
                text(data.day, startX, height/2 + 20);
            }
            
            // 标题
            fill(255);
            textSize(20);
            textAlign(CENTER);
            text("数据流动 - 一周轨迹", width/2, 50);
        }

        function keyPressed() {
            if (key === 's' || key === 'S') {
                saveCanvas('organic-art', 'png');
            }
        }
    </script>
</body>
</html>
```

---

### Step 4: 创建海报设计

创建文件：`posters/poster-template.html`

```html
<!DOCTYPE html>
<html lang="zh">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>数据可视化海报</title>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Poppins:wght@700&family=Inter:wght@400;600&display=swap');
        
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        
        body {
            font-family: 'Inter', sans-serif;
            background: #f5f5f5;
            padding: 20px;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
        }
        
        .poster {
            width: 800px;
            height: 1131px; /* A4比例 */
            background: white;
            box-shadow: 0 20px 60px rgba(0,0,0,0.15);
            position: relative;
            overflow: hidden;
        }
        
        .header {
            padding: 60px;
            background: linear-gradient(135deg, #2c3e50 0%, #34495e 100%);
            color: white;
        }
        
        .header h1 {
            font-family: 'Poppins', sans-serif;
            font-size: 48px;
            margin-bottom: 10px;
        }
        
        .header p {
            font-size: 18px;
            opacity: 0.9;
        }
        
        .art-container {
            padding: 40px;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 600px;
        }
        
        .art-placeholder {
            width: 600px;
            height: 600px;
            background: linear-gradient(45deg, #3498db, #9b59b6);
            border-radius: 20px;
            display: flex;
            justify-content: center;
            align-items: center;
            color: white;
            font-size: 24px;
        }
        
        .data-summary {
            padding: 40px 60px;
            background: #ecf0f1;
        }
        
        .summary-title {
            font-family: 'Poppins', sans-serif;
            font-size: 24px;
            color: #2c3e50;
            margin-bottom: 20px;
        }
        
        .stats {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 20px;
        }
        
        .stat-card {
            background: white;
            padding: 20px;
            border-radius: 10px;
            text-align: center;
        }
        
        .stat-value {
            font-size: 32px;
            font-weight: 600;
            color: #3498db;
            margin-bottom: 5px;
        }
        
        .stat-label {
            font-size: 14px;
            color: #7f8c8d;
        }
        
        .footer {
            padding: 30px 60px;
            text-align: center;
            color: #7f8c8d;
            font-size: 12px;
        }
    </style>
</head>
<body>
    <div class="poster">
        <!-- 标题区域 -->
        <div class="header">
            <h1>我的一周</h1>
            <p>数据驱动的生活艺术</p>
        </div>
        
        <!-- 艺术作品区域 -->
        <div class="art-container">
            <div class="art-placeholder">
                在这里插入你的算法艺术图片
            </div>
        </div>
        
        <!-- 数据摘要 -->
        <div class="data-summary">
            <div class="summary-title">本周数据洞察</div>
            <div class="stats">
                <div class="stat-card">
                    <div class="stat-value">62,081</div>
                    <div class="stat-label">总步数</div>
                </div>
                <div class="stat-card">
                    <div class="stat-value">7.7</div>
                    <div class="stat-label">平均心情</div>
                </div>
                <div class="stat-card">
                    <div class="stat-value">6.7</div>
                    <div class="stat-label">平均生产力</div>
                </div>
            </div>
        </div>
        
        <!-- 页脚 -->
        <div class="footer">
            数据可视化艺术 | 2025年12月 | 使用 p5.js 创作
        </div>
    </div>
    
    <script>
        // 说明：右键点击海报可以保存为图片
        // 实际使用时，将算法艺术导出的图片替换 art-placeholder
    </script>
</body>
</html>
```

---

## 🚀 使用步骤

### 1. 打开几何风格艺术

```bash
# 用浏览器打开
art/geometric/index.html

# 按 's' 键保存图片
```

### 2. 打开有机风格艺术

```bash
art/organic/index.html

# 按 's' 键保存图片
```

### 3. 创建海报

1. 打开 `posters/poster-template.html`
2. 将保存的艺术图片插入到海报中
3. 调整文字和数据
4. 右键保存整个页面为图片

---

## 💡 在Cursor中优化

### 让Claude帮你改进艺术作品

选中p5.js代码，按Ctrl+K：

```markdown
"请帮我改进这个算法艺术：
1. 让动画更生动（添加动画效果）
2. 使用更好的配色方案
3. 添加交互功能（鼠标悬停显示详情）
4. 优化数据映射，让视觉更有冲击力"
```

---

## 📂 完整项目结构

```
data-visualization-art/
├── README.md
├── SAMPLE.md (本文件)
├── data/
│   └── sample_data.json ✅
├── art/
│   ├── geometric/
│   │   └── index.html ✅
│   ├── organic/
│   │   └── index.html ✅
│   └── fractal/
│       └── index.html (待创建)
└── posters/
    ├── poster-template.html ✅
    └── final-posters/
        ├── geometric-poster.png
        └── organic-poster.png
```

---

## ✅ 检查清单

- [ ] 成功在浏览器中打开几何风格艺术
- [ ] 成功在浏览器中打开有机风格艺术
- [ ] 保存了艺术作品为PNG图片
- [ ] 打开了海报模板
- [ ] 自定义了海报中的数据和图片
- [ ] 完成了至少2个艺术作品和1张海报

---

## 🎨 扩展练习

1. **添加第三种风格**：创建分形风格的艺术
2. **使用真实数据**：导入你自己的数据（GitHub、运动等）
3. **制作动画版本**：使用 `draw()` 函数添加动画
4. **交互版本**：添加鼠标交互、滑块控制参数

---

**开始创作你的数据艺术吧！🎨**

