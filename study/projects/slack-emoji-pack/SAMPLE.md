# 项目3示例：Slack团队表情包

这是一个完整的Python示例，展示如何创建你的第一个Slack GIF表情。

---

## 🎯 快速开始：创建第一个GIF

### Step 1: 环境准备

```bash
# 1. 创建项目目录
cd study/projects/slack-emoji-pack

# 2. 创建虚拟环境
python -m venv venv

# Windows激活:
venv\Scripts\activate

# Mac/Linux激活:
source venv/bin/activate

# 3. 安装依赖
pip install Pillow
```

---

### Step 2: 创建核心工具

#### 2.1 简化版 GIF Builder

创建文件：`simple_gif_builder.py`

```python
"""
简化版GIF构建器
用于快速创建Slack表情GIF
"""

from PIL import Image, ImageDraw, ImageFont
import os

class SimpleGIFBuilder:
    """简单的GIF构建器"""
    
    def __init__(self, width=128, height=128, fps=12):
        """
        初始化GIF构建器
        
        Args:
            width: 图像宽度（像素）
            height: 图像高度（像素）
            fps: 帧率（每秒帧数）
        """
        self.width = width
        self.height = height
        self.fps = fps
        self.frames = []
        self.duration = int(1000 / fps)  # 每帧持续时间（毫秒）
    
    def add_frame(self, frame):
        """添加一帧到GIF"""
        self.frames.append(frame)
    
    def save(self, filename, loop=0):
        """
        保存GIF文件
        
        Args:
            filename: 输出文件名
            loop: 循环次数（0表示无限循环）
        """
        if not self.frames:
            raise ValueError("没有帧可以保存")
        
        # 确保输出目录存在
        os.makedirs(os.path.dirname(filename) if os.path.dirname(filename) else '.', exist_ok=True)
        
        # 保存GIF
        self.frames[0].save(
            filename,
            save_all=True,
            append_images=self.frames[1:],
            duration=self.duration,
            loop=loop,
            optimize=True
        )
        
        # 打印文件信息
        file_size = os.path.getsize(filename) / 1024  # KB
        print(f"✅ GIF已保存: {filename}")
        print(f"   尺寸: {self.width}x{self.height}px")
        print(f"   帧数: {len(self.frames)}帧")
        print(f"   文件大小: {file_size:.1f} KB")
        
        if file_size > 100:
            print(f"   ⚠️  警告：文件大于100KB，建议优化")

# 缓动函数
def ease_out_bounce(t):
    """弹跳缓动函数"""
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

def ease_in_out(t):
    """缓入缓出函数"""
    return t * t * (3 - 2 * t)
```

---

### Step 3: 创建第一个表情 - 👍 点赞

创建文件：`create_thumbs_up.py`

```python
"""
创建点赞表情GIF
"""

from PIL import Image, ImageDraw, ImageFont
from simple_gif_builder import SimpleGIFBuilder, ease_out_bounce

def create_thumbs_up_gif():
    """创建点赞动画GIF"""
    
    # 创建GIF构建器
    builder = SimpleGIFBuilder(width=128, height=128, fps=12)
    
    # 动画参数
    total_frames = 18  # 1.5秒动画
    emoji = "👍"
    emoji_size = 64
    
    # 尝试加载字体（如果失败则使用默认字体）
    try:
        font = ImageFont.truetype("seguiemj.ttf", emoji_size)  # Windows Emoji字体
    except:
        try:
            font = ImageFont.truetype("Arial.ttf", emoji_size)
        except:
            font = ImageFont.load_default()
    
    # 生成每一帧
    for i in range(total_frames):
        # 创建透明背景
        frame = Image.new('RGBA', (128, 128), (255, 255, 255, 0))
        draw = ImageDraw.Draw(frame)
        
        # 计算动画进度（0到1）
        progress = i / (total_frames - 1)
        
        # 使用弹跳缓动
        bounce_progress = ease_out_bounce(progress)
        
        # 计算位置（从底部向上弹出）
        start_y = 128
        end_y = 32
        y_pos = start_y - int((start_y - end_y) * bounce_progress)
        
        # 计算透明度（渐现）
        alpha = int(255 * min(progress * 2, 1))
        
        # 绘制表情（简化：使用文字）
        # 注意：实际效果取决于系统字体支持
        text_bbox = draw.textbbox((0, 0), emoji, font=font)
        text_width = text_bbox[2] - text_bbox[0]
        text_height = text_bbox[3] - text_bbox[1]
        x_pos = (128 - text_width) // 2
        
        # 绘制emoji
        draw.text((x_pos, y_pos), emoji, font=font, fill=(0, 0, 0, alpha))
        
        # 添加到帧列表
        builder.add_frame(frame)
    
    # 保存GIF
    builder.save('output/thumbs-up.gif', loop=0)
    print("\n🎉 点赞表情创建完成！")
    print("   可以在 output/thumbs-up.gif 查看")

if __name__ == "__main__":
    # 创建输出目录
    import os
    os.makedirs('output', exist_ok=True)
    
    # 创建GIF
    create_thumbs_up_gif()
```

---

### Step 4: 创建第二个表情 - ⏳ 加载中

创建文件：`create_loading.py`

```python
"""
创建加载中表情GIF
"""

from PIL import Image, ImageDraw
import math
from simple_gif_builder import SimpleGIFBuilder

def create_loading_gif():
    """创建旋转加载动画"""
    
    builder = SimpleGIFBuilder(width=128, height=128, fps=12)
    
    # 动画参数
    total_frames = 24  # 2秒循环
    center_x, center_y = 64, 64
    radius = 35
    dot_radius = 6
    num_dots = 8
    
    # 颜色（浅蓝到深蓝的渐变）
    colors = [
        (52, 152, 219, int(255 * (i + 1) / num_dots))  # RGB + Alpha
        for i in range(num_dots)
    ]
    
    for frame_num in range(total_frames):
        # 创建透明背景
        frame = Image.new('RGBA', (128, 128), (255, 255, 255, 0))
        draw = ImageDraw.Draw(frame)
        
        # 计算旋转角度
        base_angle = (frame_num / total_frames) * 360
        
        # 绘制8个圆点
        for i in range(num_dots):
            angle = math.radians(base_angle + i * 45)
            
            # 计算圆点位置
            x = center_x + int(radius * math.cos(angle))
            y = center_y + int(radius * math.sin(angle))
            
            # 绘制圆点
            draw.ellipse(
                [x - dot_radius, y - dot_radius, 
                 x + dot_radius, y + dot_radius],
                fill=colors[i]
            )
        
        builder.add_frame(frame)
    
    # 保存为无限循环
    builder.save('output/loading.gif', loop=0)
    print("\n⏳ 加载表情创建完成！")

if __name__ == "__main__":
    import os
    os.makedirs('output', exist_ok=True)
    create_loading_gif()
```

---

### Step 5: 创建第三个表情 - ✅ 完成

创建文件：`create_done.py`

```python
"""
创建完成表情GIF（勾号动画）
"""

from PIL import Image, ImageDraw
from simple_gif_builder import SimpleGIFBuilder, ease_in_out

def create_done_gif():
    """创建勾号动画"""
    
    builder = SimpleGIFBuilder(width=128, height=128, fps=12)
    
    total_frames = 20  # 约1.7秒
    
    # 勾号的路径（简化版）
    checkmark_points = [
        (35, 64),   # 起点
        (55, 84),   # 转折点
        (93, 44),   # 终点
    ]
    
    for frame_num in range(total_frames):
        frame = Image.new('RGBA', (128, 128), (255, 255, 255, 0))
        draw = ImageDraw.Draw(frame)
        
        # 动画分两段
        if frame_num < 10:
            # 第一段：画勾号的第一笔
            progress = ease_in_out(frame_num / 10)
            x1, y1 = checkmark_points[0]
            x2, y2 = checkmark_points[1]
            
            current_x = x1 + (x2 - x1) * progress
            current_y = y1 + (y2 - y1) * progress
            
            draw.line([x1, y1, current_x, current_y], 
                     fill=(46, 204, 113, 255), width=8)
        
        elif frame_num < 20:
            # 画完第一笔
            draw.line([checkmark_points[0], checkmark_points[1]], 
                     fill=(46, 204, 113, 255), width=8)
            
            # 第二段：画第二笔
            progress = ease_in_out((frame_num - 10) / 10)
            x1, y1 = checkmark_points[1]
            x2, y2 = checkmark_points[2]
            
            current_x = x1 + (x2 - x1) * progress
            current_y = y1 + (y2 - y1) * progress
            
            draw.line([x1, y1, current_x, current_y], 
                     fill=(46, 204, 113, 255), width=8)
        
        # 添加圆形背景（淡绿色）
        if frame_num >= 15:
            alpha = int(100 * (frame_num - 15) / 5)
            draw.ellipse([24, 24, 104, 104], 
                        fill=(46, 204, 113, alpha))
        
        builder.add_frame(frame)
    
    builder.save('output/done.gif', loop=1)  # 只播放一次
    print("\n✅ 完成表情创建完成！")

if __name__ == "__main__":
    import os
    os.makedirs('output', exist_ok=True)
    create_done_gif()
```

---

### Step 6: 创建批量生成脚本

创建文件：`create_all.py`

```python
"""
批量创建所有表情
"""

import os
from create_thumbs_up import create_thumbs_up_gif
from create_loading import create_loading_gif
from create_done import create_done_gif

def main():
    """创建所有表情"""
    
    print("=" * 50)
    print("🎨 开始创建Slack表情包...")
    print("=" * 50)
    
    # 确保输出目录存在
    os.makedirs('output', exist_ok=True)
    
    # 创建各个表情
    print("\n[1/3] 创建点赞表情...")
    create_thumbs_up_gif()
    
    print("\n[2/3] 创建加载表情...")
    create_loading_gif()
    
    print("\n[3/3] 创建完成表情...")
    create_done_gif()
    
    print("\n" + "=" * 50)
    print("🎉 所有表情创建完成！")
    print("=" * 50)
    print("\n📁 输出目录: output/")
    print("   - thumbs-up.gif")
    print("   - loading.gif")
    print("   - done.gif")
    print("\n💡 提示：")
    print("   1. 在浏览器中打开GIF文件预览")
    print("   2. 如果文件过大，可以减少帧数或颜色数")
    print("   3. 上传到Slack测试效果")

if __name__ == "__main__":
    main()
```

---

## 🚀 运行示例

### 方法1：创建单个表情

```bash
# 激活虚拟环境
venv\Scripts\activate  # Windows
# 或
source venv/bin/activate  # Mac/Linux

# 创建点赞表情
python create_thumbs_up.py

# 创建加载表情
python create_loading.py

# 创建完成表情
python create_done.py
```

### 方法2：批量创建

```bash
python create_all.py
```

---

## 📂 完整项目结构

```
slack-emoji-pack/
├── README.md
├── SAMPLE.md (本文件)
├── simple_gif_builder.py ✅
├── create_thumbs_up.py ✅
├── create_loading.py ✅
├── create_done.py ✅
├── create_all.py ✅
├── output/
│   ├── thumbs-up.gif
│   ├── loading.gif
│   └── done.gif
└── venv/ (虚拟环境)
```

---

## 🎨 自定义你的表情

### 修改颜色

```python
# 在代码中修改颜色值
# RGB格式
color = (52, 152, 219, 255)  # 蓝色
#        R   G   B   Alpha

# 常用颜色
red = (231, 76, 60, 255)
green = (46, 204, 113, 255)
blue = (52, 152, 219, 255)
yellow = (241, 196, 15, 255)
purple = (155, 89, 182, 255)
```

### 调整大小和帧率

```python
# 在SimpleGIFBuilder初始化时
builder = SimpleGIFBuilder(
    width=128,   # 宽度（像素）
    height=128,  # 高度（像素）
    fps=12       # 帧率（建议10-15）
)
```

### 修改动画时长

```python
# 改变总帧数
total_frames = 24  # 2秒 (24帧 / 12fps)
total_frames = 12  # 1秒
total_frames = 36  # 3秒
```

---

## 🔍 在Cursor中优化

### 让Claude帮你优化

```markdown
选中你的Python代码，按 Ctrl+K，输入：

"请帮我优化这个GIF动画：
1. 减小文件大小（目标<100KB）
2. 让动画更流畅
3. 添加更有趣的效果
4. 建议颜色搭配"
```

---

## ✅ 检查清单

- [ ] 成功创建了虚拟环境
- [ ] 安装了Pillow库
- [ ] 运行了3个示例脚本
- [ ] 生成的GIF可以正常显示
- [ ] 文件大小<100KB
- [ ] 在浏览器中预览效果满意

---

## 💡 下一步

1. **创建更多表情**：参考README.md中的10个表情清单
2. **上传到Slack测试**：看实际效果
3. **优化文件大小**：如果>100KB，减少帧数或颜色
4. **添加特效**：阴影、发光、粒子效果

---

**开始创建你的Slack表情包吧！🎨**

