# 项目5：数据可视化仪表板

**所属阶段**：第四阶段（第6-7周）  
**状态**：⏸️ 未开始  
**难度**：⭐⭐⭐⭐

---

## 📋 项目目标

创建一个完整的数据可视化Web应用，包含前端界面、数据可视化、自动化测试和专业UI设计。

---

## 🎯 学习目标

通过本项目，你将：
- ✅ 掌握React + TypeScript开发
- ✅ 学会使用shadcn/ui组件库
- ✅ 能够创建交互式数据可视化
- ✅ 掌握Playwright自动化测试
- ✅ 应用专业的frontend设计原则

---

## 📦 涉及的Skills

1. **web-artifacts-builder** - Web构件构建器
2. **webapp-testing** - Web应用测试
3. **frontend-design** - 前端界面设计

---

## ✅ 实施步骤

### Part 1: 项目初始化

#### 1.1 使用web-artifacts-builder创建项目
```bash
# 研究文档
study ../../../anthropics-skills/skills/web-artifacts-builder/SKILL.md

# 使用初始化脚本（如果在Linux/Mac）
bash ../../../anthropics-skills/skills/web-artifacts-builder/scripts/init-artifact.sh

# 或手动创建Vite + React项目
npm create vite@latest data-dashboard -- --template react-ts
cd data-dashboard
npm install
```

#### 1.2 安装必要依赖
```bash
# 安装shadcn/ui
npx shadcn-ui@latest init

# 安装其他依赖
npm install @tanstack/react-query axios recharts
npm install -D @playwright/test
```

---

### Part 2: 设计系统架构

#### 2.1 数据源设计
使用第三阶段（项目4）的Excel数据作为数据源。

**数据类型**：
- 财务数据（收入、支出、利润）
- 销售数据（产品、地区、时间）
- 用户数据（活跃度、增长）

#### 2.2 页面结构
```
Dashboard
├── Header（导航栏）
├── Sidebar（侧边栏）
└── Main Content
    ├── Overview（总览）
    ├── Charts（图表页）
    ├── Data Table（数据表格）
    └── Settings（设置）
```

---

### Part 3: 实现核心功能

#### 3.1 创建布局组件

**App.tsx**
```typescript
import { BrowserRouter, Routes, Route } from 'react-router-dom';
import { Layout } from '@/components/Layout';
import { Overview } from '@/pages/Overview';
import { Charts } from '@/pages/Charts';
import { DataTable } from '@/pages/DataTable';

function App() {
  return (
    <BrowserRouter>
      <Layout>
        <Routes>
          <Route path="/" element={<Overview />} />
          <Route path="/charts" element={<Charts />} />
          <Route path="/data" element={<DataTable />} />
        </Routes>
      </Layout>
    </BrowserRouter>
  );
}
```

**Layout组件**
```typescript
// src/components/Layout.tsx
import { Header } from './Header';
import { Sidebar } from './Sidebar';

export function Layout({ children }: { children: React.ReactNode }) {
  return (
    <div className="flex h-screen">
      <Sidebar />
      <div className="flex-1 flex flex-col">
        <Header />
        <main className="flex-1 overflow-auto p-6 bg-gray-50">
          {children}
        </main>
      </div>
    </div>
  );
}
```

#### 3.2 集成shadcn/ui组件

安装需要的组件：
```bash
npx shadcn-ui@latest add button
npx shadcn-ui@latest add card
npx shadcn-ui@latest add table
npx shadcn-ui@latest add tabs
npx shadcn-ui@latest add select
npx shadcn-ui@latest add dialog
```

**使用示例**：
```typescript
import { Card, CardContent, CardHeader, CardTitle } from '@/components/ui/card';

export function StatCard({ title, value, change }: StatCardProps) {
  return (
    <Card>
      <CardHeader>
        <CardTitle>{title}</CardTitle>
      </CardHeader>
      <CardContent>
        <div className="text-2xl font-bold">{value}</div>
        <div className="text-sm text-gray-500">{change}</div>
      </CardContent>
    </Card>
  );
}
```

#### 3.3 实现数据可视化

**使用Recharts创建图表**：
```typescript
// src/components/charts/LineChart.tsx
import { LineChart, Line, XAxis, YAxis, CartesianGrid, Tooltip, Legend } from 'recharts';

export function RevenueChart({ data }: { data: any[] }) {
  return (
    <LineChart width={600} height={300} data={data}>
      <CartesianGrid strokeDasharray="3 3" />
      <XAxis dataKey="month" />
      <YAxis />
      <Tooltip />
      <Legend />
      <Line type="monotone" dataKey="revenue" stroke="#8884d8" />
      <Line type="monotone" dataKey="profit" stroke="#82ca9d" />
    </LineChart>
  );
}
```

**图表类型**：
- [ ] 折线图（趋势分析）
- [ ] 柱状图（对比分析）
- [ ] 饼图（占比分析）
- [ ] 面积图（累积分析）
- [ ] 散点图（相关性分析）

#### 3.4 实现数据筛选功能

```typescript
// src/components/DataFilter.tsx
import { Select } from '@/components/ui/select';
import { DateRangePicker } from '@/components/ui/date-range-picker';

export function DataFilter({ onFilterChange }: DataFilterProps) {
  const [dateRange, setDateRange] = useState<DateRange>();
  const [category, setCategory] = useState<string>('all');

  return (
    <div className="flex gap-4">
      <Select value={category} onValueChange={setCategory}>
        <option value="all">所有类别</option>
        <option value="sales">销售</option>
        <option value="marketing">营销</option>
      </Select>
      
      <DateRangePicker value={dateRange} onChange={setDateRange} />
      
      <Button onClick={() => onFilterChange({ category, dateRange })}>
        应用筛选
      </Button>
    </div>
  );
}
```

---

### Part 4: 应用Frontend Design原则

参考 `frontend-design/SKILL.md`：

#### 4.1 排版（Typography）
```css
/* 定义字体系统 */
:root {
  --font-sans: 'Inter', sans-serif;
  --font-heading: 'Poppins', sans-serif;
  
  --text-xs: 0.75rem;
  --text-sm: 0.875rem;
  --text-base: 1rem;
  --text-lg: 1.125rem;
  --text-xl: 1.25rem;
  --text-2xl: 1.5rem;
}
```

#### 4.2 色彩系统
```typescript
// tailwind.config.js
export default {
  theme: {
    extend: {
      colors: {
        primary: {
          50: '#f0f9ff',
          100: '#e0f2fe',
          // ...
          900: '#0c4a6e',
        },
      },
    },
  },
};
```

#### 4.3 空间和布局
- 使用8px网格系统
- 保持一致的间距（padding、margin）
- 响应式设计（mobile-first）

#### 4.4 动效
```typescript
// 使用Framer Motion
import { motion } from 'framer-motion';

export function Card({ children }: { children: React.ReactNode }) {
  return (
    <motion.div
      initial={{ opacity: 0, y: 20 }}
      animate={{ opacity: 1, y: 0 }}
      transition={{ duration: 0.3 }}
      className="bg-white rounded-lg shadow p-6"
    >
      {children}
    </motion.div>
  );
}
```

---

### Part 5: 编写自动化测试

使用Playwright编写测试。

#### 5.1 设置测试环境
```typescript
// playwright.config.ts
import { defineConfig } from '@playwright/test';

export default defineConfig({
  testDir: './tests',
  use: {
    baseURL: 'http://localhost:5173',
    screenshot: 'only-on-failure',
  },
  webServer: {
    command: 'npm run dev',
    port: 5173,
  },
});
```

#### 5.2 编写测试用例

**测试导航**：
```typescript
// tests/navigation.spec.ts
import { test, expect } from '@playwright/test';

test('should navigate between pages', async ({ page }) => {
  await page.goto('/');
  
  // 检查主页加载
  await expect(page.locator('h1')).toContainText('Dashboard');
  
  // 点击图表页
  await page.click('text=Charts');
  await expect(page).toHaveURL('/charts');
  
  // 检查图表渲染
  await expect(page.locator('.recharts-wrapper')).toBeVisible();
});
```

**测试数据筛选**：
```typescript
// tests/filter.spec.ts
test('should filter data correctly', async ({ page }) => {
  await page.goto('/data');
  
  // 选择类别
  await page.selectOption('select#category', 'sales');
  
  // 应用筛选
  await page.click('button:has-text("应用筛选")');
  
  // 等待数据更新
  await page.waitForTimeout(500);
  
  // 验证结果
  const rows = await page.locator('table tbody tr').count();
  expect(rows).toBeGreaterThan(0);
});
```

**测试图表交互**：
```typescript
// tests/charts.spec.ts
test('should display chart tooltip on hover', async ({ page }) => {
  await page.goto('/charts');
  
  // 悬停在图表数据点
  await page.hover('.recharts-line');
  
  // 检查tooltip显示
  await expect(page.locator('.recharts-tooltip-wrapper')).toBeVisible();
});
```

---

### Part 6: 性能优化

- [ ] 使用React.memo优化组件重渲染
- [ ] 实现虚拟滚动（大数据表格）
- [ ] 代码分割（React.lazy）
- [ ] 图片优化
- [ ] 使用Web Workers处理大数据

```typescript
// 使用React.memo
export const ChartCard = React.memo(({ data }: ChartCardProps) => {
  return <Card>...</Card>;
});

// 代码分割
const Charts = React.lazy(() => import('@/pages/Charts'));

function App() {
  return (
    <Suspense fallback={<Loading />}>
      <Charts />
    </Suspense>
  );
}
```

---

## 📂 项目结构

```
data-dashboard/
├── README.md
├── package.json
├── tsconfig.json
├── vite.config.ts
├── tailwind.config.js
├── playwright.config.ts
├── src/
│   ├── main.tsx
│   ├── App.tsx
│   ├── components/
│   │   ├── ui/              # shadcn/ui组件
│   │   ├── Layout.tsx
│   │   ├── Header.tsx
│   │   ├── Sidebar.tsx
│   │   ├── charts/          # 图表组件
│   │   └── DataFilter.tsx
│   ├── pages/
│   │   ├── Overview.tsx
│   │   ├── Charts.tsx
│   │   ├── DataTable.tsx
│   │   └── Settings.tsx
│   ├── lib/
│   │   ├── api.ts           # API调用
│   │   └── utils.ts
│   ├── hooks/               # 自定义Hooks
│   ├── types/               # TypeScript类型
│   └── styles/
├── public/
├── tests/                   # Playwright测试
│   ├── navigation.spec.ts
│   ├── filter.spec.ts
│   └── charts.spec.ts
└── docs/
    ├── deployment.md
    └── api-docs.md
```

---

## 📝 交付物清单

- [ ] **完整的Web应用（React + TypeScript）** - 可运行的仪表板
- [ ] **Playwright测试套件** - 至少10个测试用例
- [ ] **部署包和文档** - 部署说明和使用手册

---

## 🔧 技术栈

- **前端框架**：React 18 + TypeScript
- **构建工具**：Vite
- **UI库**：shadcn/ui + Tailwind CSS
- **图表库**：Recharts
- **状态管理**：TanStack Query
- **路由**：React Router
- **测试**：Playwright
- **动画**：Framer Motion（可选）

---

## 💡 开发技巧

### 项目最佳实践
1. 使用TypeScript严格模式
2. 组件化开发，单一职责
3. 使用Hooks抽象逻辑
4. 编写测试覆盖核心功能
5. 注重可访问性（a11y）

### 常见问题解决
- **图表不显示**：检查数据格式和Recharts配置
- **样式不生效**：确保Tailwind配置正确
- **测试失败**：检查选择器和等待时间

---

## 🔗 相关资源

### Skills文档
- [web-artifacts-builder](../../../anthropics-skills/skills/web-artifacts-builder/SKILL.md)
- [webapp-testing](../../../anthropics-skills/skills/webapp-testing/SKILL.md)
- [frontend-design](../../../anthropics-skills/skills/frontend-design/SKILL.md)

### 学习笔记
- [第6-7周学习笔记](../../learning-notes/week-6-7-web.md)

### 官方文档
- [shadcn/ui](https://ui.shadcn.com/)
- [Recharts](https://recharts.org/)
- [Playwright](https://playwright.dev/)

---

## ✨ 开始项目

### 快速开始
```bash
# 1. 创建项目
npm create vite@latest data-dashboard -- --template react-ts

# 2. 安装依赖
cd data-dashboard
npm install

# 3. 初始化shadcn/ui
npx shadcn-ui@latest init

# 4. 启动开发服务器
npm run dev

# 5. 在Cursor中让Claude帮你开发功能
```

---

**预计完成时间**：7天（第6-7周的一部分）  
**开始日期**：  
**完成日期**：

