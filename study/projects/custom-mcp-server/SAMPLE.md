# 项目6示例：自定义MCP服务器

这个示例展示如何用Python创建一个简单的MCP服务器。

---

## 🎯 快速开始：15分钟创建MCP服务器

### Step 1: 环境准备

```bash
cd study/projects/custom-mcp-server

# 创建虚拟环境
python -m venv venv
venv\Scripts\activate  # Windows

# 安装MCP SDK
pip install mcp
```

---

### Step 2: 创建简单的MCP服务器

创建文件：`simple_server.py`

```python
"""
简单的MCP服务器示例
提供文本处理工具
"""

import asyncio
from mcp.server import Server
from mcp.server.models import InitializationOptions
import mcp.server.stdio
import mcp.types as types

# 创建服务器实例
app = Server("simple-text-tools")

# 定义工具列表
@app.list_tools()
async def handle_list_tools() -> list[types.Tool]:
    """
    返回服务器提供的工具列表
    """
    return [
        types.Tool(
            name="count_words",
            description="统计文本中的单词数量",
            inputSchema={
                "type": "object",
                "properties": {
                    "text": {
                        "type": "string",
                        "description": "要统计的文本",
                    },
                },
                "required": ["text"],
            },
        ),
        types.Tool(
            name="reverse_text",
            description="反转文本内容",
            inputSchema={
                "type": "object",
                "properties": {
                    "text": {
                        "type": "string",
                        "description": "要反转的文本",
                    },
                },
                "required": ["text"],
            },
        ),
        types.Tool(
            name="to_uppercase",
            description="将文本转换为大写",
            inputSchema={
                "type": "object",
                "properties": {
                    "text": {
                        "type": "string",
                        "description": "要转换的文本",
                    },
                },
                "required": ["text"],
            },
        ),
    ]

# 实现工具调用
@app.call_tool()
async def handle_call_tool(
    name: str, arguments: dict
) -> list[types.TextContent]:
    """
    处理工具调用请求
    """
    
    if name == "count_words":
        text = arguments["text"]
        word_count = len(text.split())
        return [
            types.TextContent(
                type="text",
                text=f"文本包含 {word_count} 个单词。"
            )
        ]
    
    elif name == "reverse_text":
        text = arguments["text"]
        reversed_text = text[::-1]
        return [
            types.TextContent(
                type="text",
                text=f"反转后的文本：{reversed_text}"
            )
        ]
    
    elif name == "to_uppercase":
        text = arguments["text"]
        upper_text = text.upper()
        return [
            types.TextContent(
                type="text",
                text=f"大写文本：{upper_text}"
            )
        ]
    
    else:
        raise ValueError(f"未知的工具: {name}")

# 主函数
async def main():
    """启动MCP服务器"""
    async with mcp.server.stdio.stdio_server() as (read_stream, write_stream):
        await app.run(
            read_stream,
            write_stream,
            InitializationOptions(
                server_name="simple-text-tools",
                server_version="1.0.0",
                capabilities=app.get_capabilities(
                    notification_options=types.NotificationOptions(),
                    experimental_capabilities={},
                ),
            ),
        )

if __name__ == "__main__":
    print("启动Simple Text Tools MCP服务器...", flush=True)
    asyncio.run(main())
```

---

### Step 3: 更完整的示例 - 文件处理服务器

创建文件：`file_tools_server.py`

```python
"""
文件处理MCP服务器
提供文件读取、写入、分析等功能
"""

import asyncio
import os
from pathlib import Path
from mcp.server import Server
from mcp.server.models import InitializationOptions
import mcp.server.stdio
import mcp.types as types

app = Server("file-tools")

@app.list_tools()
async def handle_list_tools() -> list[types.Tool]:
    """文件处理工具列表"""
    return [
        types.Tool(
            name="read_file",
            description="读取文件内容",
            inputSchema={
                "type": "object",
                "properties": {
                    "path": {
                        "type": "string",
                        "description": "文件路径",
                    },
                },
                "required": ["path"],
            },
        ),
        types.Tool(
            name="write_file",
            description="写入文件内容",
            inputSchema={
                "type": "object",
                "properties": {
                    "path": {
                        "type": "string",
                        "description": "文件路径",
                    },
                    "content": {
                        "type": "string",
                        "description": "文件内容",
                    },
                },
                "required": ["path", "content"],
            },
        ),
        types.Tool(
            name="list_files",
            description="列出目录中的文件",
            inputSchema={
                "type": "object",
                "properties": {
                    "directory": {
                        "type": "string",
                        "description": "目录路径",
                    },
                },
                "required": ["directory"],
            },
        ),
        types.Tool(
            name="file_stats",
            description="获取文件统计信息",
            inputSchema={
                "type": "object",
                "properties": {
                    "path": {
                        "type": "string",
                        "description": "文件路径",
                    },
                },
                "required": ["path"],
            },
        ),
    ]

@app.call_tool()
async def handle_call_tool(
    name: str, arguments: dict
) -> list[types.TextContent]:
    """处理工具调用"""
    
    try:
        if name == "read_file":
            path = arguments["path"]
            
            if not os.path.exists(path):
                return [types.TextContent(
                    type="text",
                    text=f"❌ 文件不存在: {path}"
                )]
            
            with open(path, 'r', encoding='utf-8') as f:
                content = f.read()
            
            return [types.TextContent(
                type="text",
                text=f"✅ 文件内容：\n\n{content}"
            )]
        
        elif name == "write_file":
            path = arguments["path"]
            content = arguments["content"]
            
            # 确保目录存在
            os.makedirs(os.path.dirname(path) if os.path.dirname(path) else '.', exist_ok=True)
            
            with open(path, 'w', encoding='utf-8') as f:
                f.write(content)
            
            return [types.TextContent(
                type="text",
                text=f"✅ 文件已写入: {path}"
            )]
        
        elif name == "list_files":
            directory = arguments["directory"]
            
            if not os.path.exists(directory):
                return [types.TextContent(
                    type="text",
                    text=f"❌ 目录不存在: {directory}"
                )]
            
            files = os.listdir(directory)
            files_list = "\n".join([f"- {f}" for f in files])
            
            return [types.TextContent(
                type="text",
                text=f"✅ 目录 {directory} 中的文件:\n\n{files_list}"
            )]
        
        elif name == "file_stats":
            path = arguments["path"]
            
            if not os.path.exists(path):
                return [types.TextContent(
                    type="text",
                    text=f"❌ 文件不存在: {path}"
                )]
            
            stats = os.stat(path)
            file_size = stats.st_size
            
            # 读取文件计算统计
            if os.path.isfile(path):
                with open(path, 'r', encoding='utf-8') as f:
                    content = f.read()
                    lines = len(content.split('\n'))
                    words = len(content.split())
                    chars = len(content)
                
                return [types.TextContent(
                    type="text",
                    text=f"""✅ 文件统计信息:
                    
文件: {path}
大小: {file_size} 字节
行数: {lines}
单词数: {words}
字符数: {chars}"""
                )]
            else:
                return [types.TextContent(
                    type="text",
                    text=f"✅ 目录大小: {file_size} 字节"
                )]
        
        else:
            return [types.TextContent(
                type="text",
                text=f"❌ 未知工具: {name}"
            )]
    
    except Exception as e:
        return [types.TextContent(
            type="text",
            text=f"❌ 错误: {str(e)}"
        )]

# 提供Resources（可选）
@app.list_resources()
async def handle_list_resources() -> list[types.Resource]:
    """列出可用资源"""
    return [
        types.Resource(
            uri="config://server",
            name="服务器配置",
            mimeType="application/json",
            description="MCP服务器的配置信息",
        ),
    ]

@app.read_resource()
async def handle_read_resource(uri: str) -> str:
    """读取资源"""
    if uri == "config://server":
        config = {
            "name": "file-tools",
            "version": "1.0.0",
            "supported_operations": ["read", "write", "list", "stats"]
        }
        import json
        return json.dumps(config, indent=2)
    
    raise ValueError(f"未知资源: {uri}")

async def main():
    """启动服务器"""
    async with mcp.server.stdio.stdio_server() as (read_stream, write_stream):
        await app.run(
            read_stream,
            write_stream,
            InitializationOptions(
                server_name="file-tools",
                server_version="1.0.0",
                capabilities=app.get_capabilities(
                    notification_options=types.NotificationOptions(),
                    experimental_capabilities={},
                ),
            ),
        )

if __name__ == "__main__":
    print("启动File Tools MCP服务器...", flush=True)
    asyncio.run(main())
```

---

### Step 4: 配置Claude Desktop

创建文件：`claude_desktop_config.json`

```json
{
  "mcpServers": {
    "simple-text-tools": {
      "command": "python",
      "args": [
        "D:/MCP/skills/study/projects/custom-mcp-server/simple_server.py"
      ]
    },
    "file-tools": {
      "command": "python",
      "args": [
        "D:/MCP/skills/study/projects/custom-mcp-server/file_tools_server.py"
      ]
    }
  }
}
```

> 注意：将路径替换为你的实际路径

---

### Step 5: 测试MCP服务器

创建文件：`test_server.py`

```python
"""
测试MCP服务器
"""

import subprocess
import json

def test_simple_server():
    """测试简单文本工具服务器"""
    
    print("🧪 测试Simple Text Tools服务器...")
    print()
    
    # 测试用例
    test_cases = [
        {
            "tool": "count_words",
            "args": {"text": "Hello world, this is a test"},
            "description": "统计单词"
        },
        {
            "tool": "reverse_text",
            "args": {"text": "Hello"},
            "description": "反转文本"
        },
        {
            "tool": "to_uppercase",
            "args": {"text": "hello world"},
            "description": "转大写"
        },
    ]
    
    for i, test in enumerate(test_cases, 1):
        print(f"[{i}/{len(test_cases)}] 测试: {test['description']}")
        print(f"   工具: {test['tool']}")
        print(f"   参数: {test['args']}")
        print()
    
    print("✅ 所有测试用例已定义")
    print("\n💡 提示：")
    print("   1. 在Claude Desktop中配置MCP服务器")
    print("   2. 重启Claude Desktop")
    print("   3. 在对话中使用这些工具")

if __name__ == "__main__":
    test_simple_server()
```

---

## 🚀 运行和测试

### 1. 直接运行服务器（测试）

```bash
python simple_server.py
```

### 2. 配置到Claude Desktop

将 `claude_desktop_config.json` 的内容复制到：
- Windows: `%APPDATA%\Claude\claude_desktop_config.json`
- Mac: `~/Library/Application Support/Claude/claude_desktop_config.json`

### 3. 重启Claude Desktop

### 4. 在Claude中测试

```
请使用count_words工具统计这段文本的单词数：
"Hello world, this is a test of the MCP server"
```

---

## 📂 完整项目结构

```
custom-mcp-server/
├── README.md
├── SAMPLE.md (本文件)
├── simple_server.py ✅
├── file_tools_server.py ✅
├── claude_desktop_config.json ✅
├── test_server.py ✅
└── venv/ (虚拟环境)
```

---

## ✅ 检查清单

- [ ] 成功创建了虚拟环境
- [ ] 安装了mcp包
- [ ] 创建了简单的MCP服务器
- [ ] 理解了Tool的定义和实现
- [ ] 配置了Claude Desktop（可选）
- [ ] 测试了服务器功能

---

## 💡 在Cursor中扩展

### 让Claude帮你添加更多功能

选中服务器代码，按Ctrl+K：

```markdown
"请帮我扩展这个MCP服务器：
1. 添加数据转换工具（JSON、CSV、XML）
2. 添加文本分析工具（情感分析、关键词提取）
3. 实现Resource功能（模板、配置）
4. 添加Prompt功能（预定义提示）
5. 改进错误处理和日志记录"
```

---

## 🔗 下一步

1. **添加更多工具**：根据需求扩展功能
2. **实现Resources**：提供模板和配置
3. **添加Prompts**：预定义有用的提示
4. **错误处理**：更健壮的错误处理
5. **文档完善**：编写详细的API文档
6. **测试完善**：编写单元测试

---

**开始构建你的MCP服务器吧！🛠️**

