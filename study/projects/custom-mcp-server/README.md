# 项目6：自定义MCP服务器

**所属阶段**：第四阶段（第6-7周）  
**状态**：⏸️ 未开始  
**难度**：⭐⭐⭐⭐⭐

---

## 📋 项目目标

构建一个MCP（Model Context Protocol）服务器，扩展Claude的文档处理能力，使Claude能够直接调用你的自定义工具。

---

## 🎯 学习目标

通过本项目，你将：
- ✅ 深入理解MCP协议
- ✅ 掌握TypeScript/Python的MCP SDK
- ✅ 能够实现Tools、Resources和Prompts
- ✅ 学会MCP服务器的设计和评估
- ✅ 理解MCP最佳实践

---

## 📦 涉及的Skills

1. **mcp-builder** - MCP服务器构建

---

## 🎯 项目设计：文档转换服务

### 服务器功能
创建一个"文档转换MCP服务器"，提供以下功能：

1. **文档格式转换** - 在不同格式间转换文档
2. **文档合并** - 合并多个文档
3. **文档分析** - 提取文档元数据和内容
4. **模板应用** - 应用预定义模板

---

## ✅ 实施步骤

### Step 1: 学习MCP基础

#### 1.1 阅读MCP文档
- [ ] 学习 `mcp-builder/SKILL.md`
- [ ] 阅读 `reference/mcp_best_practices.md`
- [ ] 研究 `reference/node_mcp_server.md` 或 `reference/python_mcp_server.md`

#### 1.2 理解MCP核心概念

**MCP服务器三大组件**：

1. **Tools（工具）** - 可被Claude调用的函数
2. **Resources（资源）** - 可被Claude访问的数据
3. **Prompts（提示）** - 预定义的提示模板

---

### Step 2: 选择实现语言

#### 选项A：TypeScript（推荐）
```bash
npm install @modelcontextprotocol/sdk
```

**优点**：
- 类型安全
- 社区支持好
- 与Node.js生态集成

#### 选项B：Python
```bash
pip install mcp
```

**优点**：
- 语法简洁
- 与数据处理库集成好
- 适合文档处理

---

### Step 3: 实现MCP服务器

#### 3.1 项目初始化

**TypeScript版本**：
```bash
mkdir document-converter-mcp
cd document-converter-mcp
npm init -y
npm install @modelcontextprotocol/sdk typescript @types/node
npx tsc --init
```

**Python版本**：
```bash
mkdir document-converter-mcp
cd document-converter-mcp
python -m venv venv
source venv/bin/activate  # Windows: venv\Scripts\activate
pip install mcp pandas pypdf python-docx openpyxl
```

#### 3.2 实现Tools

**TypeScript示例**：
```typescript
// src/server.ts
import { Server } from '@modelcontextprotocol/sdk/server/index.js';
import { StdioServerTransport } from '@modelcontextprotocol/sdk/server/stdio.js';
import {
  CallToolRequestSchema,
  ListToolsRequestSchema,
} from '@modelcontextprotocol/sdk/types.js';

const server = new Server(
  {
    name: 'document-converter',
    version: '1.0.0',
  },
  {
    capabilities: {
      tools: {},
    },
  }
);

// 定义工具
server.setRequestHandler(ListToolsRequestSchema, async () => {
  return {
    tools: [
      {
        name: 'convert_document',
        description: '将文档从一种格式转换为另一种格式',
        inputSchema: {
          type: 'object',
          properties: {
            input_path: {
              type: 'string',
              description: '输入文件路径',
            },
            output_format: {
              type: 'string',
              enum: ['pdf', 'docx', 'txt', 'md'],
              description: '输出格式',
            },
          },
          required: ['input_path', 'output_format'],
        },
      },
      {
        name: 'merge_documents',
        description: '合并多个文档为一个',
        inputSchema: {
          type: 'object',
          properties: {
            input_files: {
              type: 'array',
              items: { type: 'string' },
              description: '要合并的文件路径列表',
            },
            output_path: {
              type: 'string',
              description: '输出文件路径',
            },
          },
          required: ['input_files', 'output_path'],
        },
      },
    ],
  };
});

// 实现工具调用
server.setRequestHandler(CallToolRequestSchema, async (request) => {
  const { name, arguments: args } = request.params;

  if (name === 'convert_document') {
    const { input_path, output_format } = args as {
      input_path: string;
      output_format: string;
    };

    try {
      // 实现文档转换逻辑
      const result = await convertDocument(input_path, output_format);
      
      return {
        content: [
          {
            type: 'text',
            text: `文档转换成功！输出文件：${result.output_path}`,
          },
        ],
      };
    } catch (error) {
      return {
        content: [
          {
            type: 'text',
            text: `转换失败：${error.message}`,
          },
        ],
        isError: true,
      };
    }
  }

  if (name === 'merge_documents') {
    // 实现合并逻辑
    // ...
  }

  throw new Error(`Unknown tool: ${name}`);
});

// 启动服务器
async function main() {
  const transport = new StdioServerTransport();
  await server.connect(transport);
  console.error('Document Converter MCP server running on stdio');
}

main().catch((error) => {
  console.error('Server error:', error);
  process.exit(1);
});
```

**Python示例**：
```python
# server.py
from mcp.server import Server, NotificationOptions
from mcp.server.models import InitializationOptions
import mcp.server.stdio
import mcp.types as types

app = Server("document-converter")

@app.list_tools()
async def handle_list_tools() -> list[types.Tool]:
    return [
        types.Tool(
            name="convert_document",
            description="将文档从一种格式转换为另一种格式",
            inputSchema={
                "type": "object",
                "properties": {
                    "input_path": {
                        "type": "string",
                        "description": "输入文件路径",
                    },
                    "output_format": {
                        "type": "string",
                        "enum": ["pdf", "docx", "txt", "md"],
                        "description": "输出格式",
                    },
                },
                "required": ["input_path", "output_format"],
            },
        ),
        types.Tool(
            name="merge_documents",
            description="合并多个文档为一个",
            inputSchema={
                "type": "object",
                "properties": {
                    "input_files": {
                        "type": "array",
                        "items": {"type": "string"},
                        "description": "要合并的文件路径列表",
                    },
                    "output_path": {
                        "type": "string",
                        "description": "输出文件路径",
                    },
                },
                "required": ["input_files", "output_path"],
            },
        ),
    ]

@app.call_tool()
async def handle_call_tool(
    name: str, arguments: dict
) -> list[types.TextContent | types.ImageContent | types.EmbeddedResource]:
    if name == "convert_document":
        input_path = arguments["input_path"]
        output_format = arguments["output_format"]
        
        try:
            # 实现文档转换逻辑
            result = convert_document(input_path, output_format)
            
            return [
                types.TextContent(
                    type="text",
                    text=f"文档转换成功！输出文件：{result['output_path']}"
                )
            ]
        except Exception as e:
            return [
                types.TextContent(
                    type="text",
                    text=f"转换失败：{str(e)}"
                )
            ]
    
    elif name == "merge_documents":
        # 实现合并逻辑
        pass
    
    raise ValueError(f"Unknown tool: {name}")

async def main():
    async with mcp.server.stdio.stdio_server() as (read_stream, write_stream):
        await app.run(
            read_stream,
            write_stream,
            InitializationOptions(
                server_name="document-converter",
                server_version="1.0.0",
                capabilities=app.get_capabilities(
                    notification_options=NotificationOptions(),
                    experimental_capabilities={},
                ),
            ),
        )

if __name__ == "__main__":
    import asyncio
    asyncio.run(main())
```

#### 3.3 实现Resources

```typescript
// 提供可访问的资源（如模板、配置）
server.setRequestHandler(ListResourcesRequestSchema, async () => {
  return {
    resources: [
      {
        uri: 'template://report',
        name: '报告模板',
        mimeType: 'application/vnd.openxmlformats-officedocument.wordprocessingml.document',
        description: '标准报告模板',
      },
      {
        uri: 'config://converter',
        name: '转换器配置',
        mimeType: 'application/json',
        description: '文档转换器的配置选项',
      },
    ],
  };
});

server.setRequestHandler(ReadResourceRequestSchema, async (request) => {
  const { uri } = request.params;
  
  if (uri === 'template://report') {
    // 返回模板文件内容
    const templateContent = await fs.readFile('templates/report.docx');
    return {
      contents: [
        {
          uri,
          mimeType: 'application/vnd.openxmlformats-officedocument.wordprocessingml.document',
          blob: templateContent.toString('base64'),
        },
      ],
    };
  }
  
  throw new Error(`Resource not found: ${uri}`);
});
```

#### 3.4 实现Prompts

```typescript
// 提供预定义的提示模板
server.setRequestHandler(ListPromptsRequestSchema, async () => {
  return {
    prompts: [
      {
        name: 'document_summary',
        description: '生成文档摘要',
        arguments: [
          {
            name: 'document_path',
            description: '文档路径',
            required: true,
          },
        ],
      },
    ],
  };
});

server.setRequestHandler(GetPromptRequestSchema, async (request) => {
  const { name, arguments: args } = request.params;
  
  if (name === 'document_summary') {
    const docPath = args?.document_path;
    
    return {
      messages: [
        {
          role: 'user',
          content: {
            type: 'text',
            text: `请阅读文档 ${docPath} 并生成一个简洁的摘要，包括：
1. 主要内容概述
2. 关键数据点
3. 重要结论`,
          },
        },
      ],
    };
  }
  
  throw new Error(`Unknown prompt: ${name}`);
});
```

---

### Step 4: 配置MCP服务器

#### 4.1 创建配置文件

**Claude Desktop配置**（`claude_desktop_config.json`）：
```json
{
  "mcpServers": {
    "document-converter": {
      "command": "node",
      "args": ["path/to/document-converter-mcp/build/server.js"]
    }
  }
}
```

**或Python版本**：
```json
{
  "mcpServers": {
    "document-converter": {
      "command": "python",
      "args": ["path/to/document-converter-mcp/server.py"]
    }
  }
}
```

---

### Step 5: 测试和评估

#### 5.1 使用evaluation.py评估

```python
# 参考 mcp-builder/scripts/evaluation.py
from evaluation import evaluate_mcp_server

result = evaluate_mcp_server(
    server_command="node build/server.js",
    test_cases=[
        {
            "tool": "convert_document",
            "args": {
                "input_path": "test.docx",
                "output_format": "pdf"
            },
            "expected": "success"
        },
        # 更多测试用例...
    ]
)

print(f"评估结果: {result}")
```

#### 5.2 手动测试

1. 启动MCP服务器
2. 在Claude Desktop中使用
3. 测试各个功能
4. 记录问题和改进

---

## 📂 项目结构

```
custom-mcp-server/
├── README.md
├── package.json (TypeScript) 或 requirements.txt (Python)
├── tsconfig.json (TypeScript)
├── src/
│   ├── server.ts (或 server.py)
│   ├── tools/
│   │   ├── converter.ts
│   │   ├── merger.ts
│   │   └── analyzer.ts
│   ├── resources/
│   │   └── templates.ts
│   └── utils/
├── tests/
│   ├── test_tools.ts
│   └── test_integration.ts
├── docs/
│   ├── API.md
│   └── USAGE.md
└── templates/
    └── report.docx
```

---

## 📝 交付物清单

- [ ] **MCP服务器源代码（TypeScript/Python）** - 完整实现
- [ ] **配置文件和文档** - 安装和使用说明
- [ ] **测试用例和评估报告** - 功能验证

---

## 🔧 技术栈

### TypeScript
- @modelcontextprotocol/sdk
- Node.js 18+
- TypeScript 5+

### Python
- mcp
- pandas, pypdf, python-docx
- Python 3.9+

---

## 💡 MCP最佳实践

1. **清晰的工具定义** - 描述性的名称和完整的文档
2. **错误处理** - 友好的错误消息
3. **参数验证** - 验证所有输入
4. **日志记录** - 记录关键操作
5. **性能优化** - 异步处理长时间操作

---

## 🔗 相关资源

### Skills文档
- [mcp-builder](../../../anthropics-skills/skills/mcp-builder/SKILL.md)
- [MCP最佳实践](../../../anthropics-skills/skills/mcp-builder/reference/mcp_best_practices.md)
- [Node.js MCP](../../../anthropics-skills/skills/mcp-builder/reference/node_mcp_server.md)
- [Python MCP](../../../anthropics-skills/skills/mcp-builder/reference/python_mcp_server.md)

### 学习笔记
- [第6-7周学习笔记](../../learning-notes/week-6-7-web.md)

---

## ✨ 开始项目

### 快速开始（TypeScript）
```bash
mkdir document-converter-mcp && cd document-converter-mcp
npm init -y
npm install @modelcontextprotocol/sdk typescript
npx tsc --init
# 创建 src/server.ts 并开始编码
```

### 快速开始（Python）
```bash
mkdir document-converter-mcp && cd document-converter-mcp
python -m venv venv
source venv/bin/activate
pip install mcp
# 创建 server.py 并开始编码
```

---

**预计完成时间**：7天（第6-7周的一部分）  
**开始日期**：  
**完成日期**：

