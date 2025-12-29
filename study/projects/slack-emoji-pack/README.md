# 项目3：Slack团队表情包

**所属阶段**：第二阶段（第3周）  
**状态**：⏸️ 未开始  
**难度**：⭐⭐⭐

---

## 📋 项目目标

为团队或社区创建一套优化的Slack动画GIF表情包，丰富团队文化和交流方式。

---

## 🎯 学习目标

通过本项目，你将：
- ✅ 掌握GIF动画创建技巧
- ✅ 学会使用slack-gif-creator skill
- ✅ 理解GIF优化策略
- ✅ 能够创建流畅的动画效果
- ✅ 掌握缓动函数的应用

---

## 📦 涉及的Skills

1. **slack-gif-creator** - Slack GIF创建器

---

## ✅ 实施步骤

### Step 1: 学习GIF创建基础

- [ ] 阅读 `slack-gif-creator/SKILL.md`
- [ ] 研究 `core/gif_builder.py` - 核心GIF构建工具
- [ ] 学习 `core/easing.py` - 缓动函数
- [ ] 了解 `core/frame_composer.py` - 帧合成器
- [ ] 学习 `core/validators.py` - 验证工具

**关键概念**：
- 帧率（Frame Rate）：建议10-15fps
- 颜色数量：最多256色，建议64-128色
- 尺寸：Slack推荐128x128px
- 文件大小：不超过128KB（最好<100KB）

---

### Step 2: 设计表情概念（10个）

为团队设计有用且有趣的GIF表情。

#### 推荐表情类别

**工作状态类**（3个）
- [ ] 1. **thinking** 🤔 - 思考中
  - 动画：头部旋转、问号闪烁
  - 循环：无限循环
  
- [ ] 2. **coding** 💻 - 编程中
  - 动画：键盘敲击、代码滚动
  - 循环：3-5秒

- [ ] 3. **loading** ⏳ - 加载中
  - 动画：旋转、进度条
  - 循环：无限循环

**情绪反馈类**（3个）
- [ ] 4. **celebrate** 🎉 - 庆祝
  - 动画：彩纸飘落、烟花
  - 循环：2-3次

- [ ] 5. **thumbs-up** 👍 - 点赞
  - 动画：大拇指向上弹出
  - 循环：1次

- [ ] 6. **mind-blown** 🤯 - 震惊
  - 动画：爆炸效果、星星闪烁
  - 循环：1-2次

**趣味互动类**（4个）
- [ ] 7. **coffee** ☕ - 需要咖啡
  - 动画：杯子倾斜、蒸汽上升
  - 循环：无限循环

- [ ] 8. **fire** 🔥 - 太牛了
  - 动画：火焰跳动
  - 循环：无限循环

- [ ] 9. **done** ✅ - 完成
  - 动画：勾号画出、绿色闪烁
  - 循环：1次

- [ ] 10. **zzz** 😴 - 下班了
  - 动画：Z字母飘出
  - 循环：无限循环

---

### Step 3: 实现GIF动画

使用Python PIL和GIF Builder创建动画。

#### 基础代码结构

```python
from PIL import Image, ImageDraw, ImageFont
from core.gif_builder import GIFBuilder
from core.easing import ease_in_out

# 创建GIF构建器
builder = GIFBuilder(
    width=128,
    height=128,
    fps=12,
    loop=0  # 0表示无限循环
)

# 生成帧
for i in range(24):  # 2秒 (24帧 / 12fps)
    frame = Image.new('RGBA', (128, 128), (255, 255, 255, 0))
    draw = ImageDraw.Draw(frame)
    
    # 使用缓动函数计算动画进度
    progress = ease_in_out(i / 24)
    
    # 绘制动画元素
    # ... 你的绘制代码 ...
    
    builder.add_frame(frame, duration=1000/12)

# 保存GIF
builder.save('emoji/thumbs-up.gif', optimize=True)
```

#### 3.1 实现简单动画（thumbs-up）
```python
# emoji/thumbs-up.py

from PIL import Image, ImageDraw
from core.gif_builder import GIFBuilder
from core.easing import ease_out_bounce

builder = GIFBuilder(128, 128, fps=12)

for i in range(15):  # 1.25秒动画
    frame = Image.new('RGBA', (128, 128), (0, 0, 0, 0))
    draw = ImageDraw.Draw(frame)
    
    # 大拇指从下向上弹出
    progress = ease_out_bounce(i / 15)
    y_pos = 128 - int(progress * 80)
    
    # 绘制大拇指（简化版，可以用emoji或图像）
    draw.text((54, y_pos), '👍', font=ImageFont.truetype('arial.ttf', 48))
    
    builder.add_frame(frame)

builder.save('output/thumbs-up.gif', optimize=True)
```

#### 3.2 实现循环动画（loading）
```python
# emoji/loading.py

import math
from PIL import Image, ImageDraw

builder = GIFBuilder(128, 128, fps=12)

for i in range(24):  # 2秒循环
    frame = Image.new('RGBA', (128, 128), (0, 0, 0, 0))
    draw = ImageDraw.Draw(frame)
    
    # 旋转的圆圈
    angle = (i / 24) * 360
    for j in range(8):
        dot_angle = math.radians(angle + j * 45)
        x = 64 + int(40 * math.cos(dot_angle))
        y = 64 + int(40 * math.sin(dot_angle))
        alpha = int(255 * (j + 1) / 8)
        draw.ellipse([x-6, y-6, x+6, y+6], fill=(100, 149, 237, alpha))
    
    builder.add_frame(frame)

builder.save('output/loading.gif', optimize=True)
```

---

### Step 4: 优化GIF文件

确保所有GIF符合Slack的要求。

#### 优化策略

**1. 颜色优化**
```python
# 减少颜色数量
from PIL import Image

img = Image.open('emoji/original.gif')
img = img.quantize(colors=64)  # 减少到64色
img.save('emoji/optimized.gif', optimize=True)
```

**2. 尺寸优化**
- 使用128x128px（Slack推荐）
- 简化图形，减少细节
- 使用纯色背景（或透明）

**3. 帧数优化**
- 帧率：10-12fps（而不是24fps）
- 持续时间：1-3秒
- 循环：根据用途决定

**4. 压缩优化**
```python
# 使用gifsicle进一步压缩
import subprocess

subprocess.run([
    'gifsicle',
    '-O3',
    '--lossy=80',
    'input.gif',
    '-o',
    'output.gif'
])
```

**目标**：每个GIF < 100KB

---

### Step 5: 测试和完善

- [ ] 在Slack中上传测试
- [ ] 检查动画流畅度
- [ ] 确认文件大小
- [ ] 收集团队反馈
- [ ] 调整和优化

---

## 📂 项目结构

```
slack-emoji-pack/
├── README.md                    # 本文件
├── requirements.txt             # Python依赖
├── core/                        # 核心工具（从skill复制）
│   ├── gif_builder.py
│   ├── easing.py
│   ├── frame_composer.py
│   └── validators.py
├── emoji/                       # 表情源代码
│   ├── thinking.py
│   ├── coding.py
│   ├── loading.py
│   ├── celebrate.py
│   ├── thumbs-up.py
│   ├── mind-blown.py
│   ├── coffee.py
│   ├── fire.py
│   ├── done.py
│   └── zzz.py
├── output/                      # 生成的GIF（10个）
│   ├── thinking.gif
│   ├── coding.gif
│   ├── loading.gif
│   ├── celebrate.gif
│   ├── thumbs-up.gif
│   ├── mind-blown.gif
│   ├── coffee.gif
│   ├── fire.gif
│   ├── done.gif
│   └── zzz.gif
├── assets/                      # 素材资源
│   ├── fonts/
│   ├── icons/
│   └── images/
├── docs/                        # 文档
│   ├── design-guide.md         # 设计指南
│   └── usage-guide.md          # 使用说明
└── tests/                       # 测试文件
    └── preview.html            # 预览所有GIF
```

---

## 📝 交付物清单

- [ ] **10个优化的GIF文件** - 每个< 100KB
- [ ] **使用说明文档** - 如何在Slack中使用
- [ ] **创意设计文档** - 设计理念和技术说明

---

## 🎨 缓动函数参考

```python
# core/easing.py 中的常用函数

# 线性
def linear(t):
    return t

# 缓入
def ease_in(t):
    return t * t

# 缓出
def ease_out(t):
    return t * (2 - t)

# 缓入缓出
def ease_in_out(t):
    return t * t * (3 - 2 * t)

# 弹跳
def ease_out_bounce(t):
    if t < 1/2.75:
        return 7.5625 * t * t
    elif t < 2/2.75:
        t -= 1.5/2.75
        return 7.5625 * t * t + 0.75
    elif t < 2.5/2.75:
        t -= 2.25/2.75
        return 7.5625 * t * t + 0.9375
    else:
        t -= 2.625/2.75
        return 7.5625 * t * t + 0.984375

# 弹性
def ease_out_elastic(t):
    import math
    return math.pow(2, -10 * t) * math.sin((t - 0.075) * (2 * math.pi) / 0.3) + 1
```

---

## 🔧 技术要求

### Python依赖
```txt
Pillow>=10.0.0
numpy>=1.24.0
```

### 可选工具
- **gifsicle** - 进一步压缩GIF
- **ImageMagick** - 图像处理
- **Figma** - 设计原型

---

## 💡 创作技巧

### 动画设计原则
1. **简洁明了**：一眼就能理解含义
2. **循环流畅**：首尾帧要衔接自然
3. **文件优化**：牺牲一些质量换取小文件
4. **品牌一致**：使用统一的色彩和风格

### 常见动画效果
- **旋转**：loading、thinking
- **缩放**：点赞、庆祝
- **位移**：飘动、弹出
- **闪烁**：提示、强调
- **波动**：火焰、水波

---

## 🔗 相关资源

### Skills文档
- [slack-gif-creator](../../../anthropics-skills/skills/slack-gif-creator/SKILL.md)

### 核心代码
- `anthropics-skills/skills/slack-gif-creator/core/gif_builder.py`
- `anthropics-skills/skills/slack-gif-creator/core/easing.py`
- `anthropics-skills/skills/slack-gif-creator/requirements.txt`

### 学习笔记
- [第3周学习笔记](../../learning-notes/week-3-design.md)

---

## ✨ 开始项目

### 环境准备
```bash
# 安装依赖
pip install Pillow numpy

# 可选：安装gifsicle
# Windows: choco install gifsicle
# Mac: brew install gifsicle
# Linux: sudo apt-get install gifsicle
```

### 第一步：创建第一个GIF
```python
# 让Claude帮你创建第一个简单的动画
# 比如：一个点赞的大拇指

from PIL import Image, ImageDraw
from core.gif_builder import GIFBuilder

# ... 你的代码 ...
```

---

**预计完成时间**：3-4天（第3周的一部分）  
**开始日期**：  
**完成日期**：

