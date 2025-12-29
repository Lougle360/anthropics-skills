# 项目5示例：数据可视化仪表板

这个示例展示如何快速搭建一个React + TypeScript数据仪表板。

---

## 🎯 快速开始：10分钟搭建仪表板

### Step 1: 创建项目

```bash
cd study/projects/data-dashboard

# 创建Vite + React + TypeScript项目
npm create vite@latest . -- --template react-ts

# 安装依赖
npm install

# 安装额外依赖
npm install recharts @tanstack/react-query axios
npm install -D tailwindcss postcss autoprefixer
```

---

### Step 2: 配置Tailwind CSS

```bash
# 初始化Tailwind
npx tailwindcss init -p
```

编辑 `tailwind.config.js`:

```javascript
/** @type {import('tailwindcss').Config} */
export default {
  content: [
    "./index.html",
    "./src/**/*.{js,ts,jsx,tsx}",
  ],
  theme: {
    extend: {},
  },
  plugins: [],
}
```

编辑 `src/index.css`:

```css
@tailwind base;
@tailwind components;
@tailwind utilities;
```

---

### Step 3: 创建简单的仪表板

编辑 `src/App.tsx`:

```typescript
import { useState } from 'react'
import { LineChart, Line, BarChart, Bar, XAxis, YAxis, CartesianGrid, Tooltip, Legend, ResponsiveContainer } from 'recharts'

// 示例数据
const salesData = [
  { month: '1月', revenue: 45000, cost: 32000 },
  { month: '2月', revenue: 52000, cost: 35000 },
  { month: '3月', revenue: 48000, cost: 33000 },
  { month: '4月', revenue: 61000, cost: 38000 },
  { month: '5月', revenue: 55000, cost: 36000 },
  { month: '6月', revenue: 67000, cost: 40000 },
]

const productData = [
  { product: '笔记本', sales: 450 },
  { product: '鼠标', sales: 820 },
  { product: '键盘', sales: 650 },
  { product: '显示器', sales: 380 },
]

function App() {
  return (
    <div className="min-h-screen bg-gray-50">
      {/* 顶部导航 */}
      <nav className="bg-white shadow-sm">
        <div className="max-w-7xl mx-auto px-4 py-4">
          <h1 className="text-2xl font-bold text-gray-900">
            📊 数据仪表板
          </h1>
        </div>
      </nav>

      {/* 主内容 */}
      <main className="max-w-7xl mx-auto px-4 py-8">
        {/* 统计卡片 */}
        <div className="grid grid-cols-1 md:grid-cols-4 gap-6 mb-8">
          <StatCard title="总收入" value="¥328,000" change="+12.5%" positive />
          <StatCard title="总成本" value="¥214,000" change="+8.2%" positive={false} />
          <StatCard title="净利润" value="¥114,000" change="+23.1%" positive />
          <StatCard title="利润率" value="34.8%" change="+3.2%" positive />
        </div>

        {/* 图表区域 */}
        <div className="grid grid-cols-1 lg:grid-cols-2 gap-6">
          {/* 收入趋势图 */}
          <ChartCard title="收入与成本趋势">
            <ResponsiveContainer width="100%" height={300}>
              <LineChart data={salesData}>
                <CartesianGrid strokeDasharray="3 3" />
                <XAxis dataKey="month" />
                <YAxis />
                <Tooltip />
                <Legend />
                <Line type="monotone" dataKey="revenue" stroke="#3b82f6" name="收入" strokeWidth={2} />
                <Line type="monotone" dataKey="cost" stroke="#ef4444" name="成本" strokeWidth={2} />
              </LineChart>
            </ResponsiveContainer>
          </ChartCard>

          {/* 产品销量图 */}
          <ChartCard title="产品销量对比">
            <ResponsiveContainer width="100%" height={300}>
              <BarChart data={productData}>
                <CartesianGrid strokeDasharray="3 3" />
                <XAxis dataKey="product" />
                <YAxis />
                <Tooltip />
                <Bar dataKey="sales" fill="#10b981" name="销量" />
              </BarChart>
            </ResponsiveContainer>
          </ChartCard>
        </div>

        {/* 数据表格 */}
        <div className="mt-8">
          <DataTable data={salesData} />
        </div>
      </main>
    </div>
  )
}

// 统计卡片组件
interface StatCardProps {
  title: string
  value: string
  change: string
  positive: boolean
}

function StatCard({ title, value, change, positive }: StatCardProps) {
  return (
    <div className="bg-white p-6 rounded-lg shadow-sm">
      <p className="text-sm text-gray-600 mb-1">{title}</p>
      <p className="text-2xl font-bold text-gray-900 mb-2">{value}</p>
      <p className={`text-sm ${positive ? 'text-green-600' : 'text-red-600'}`}>
        {change}
      </p>
    </div>
  )
}

// 图表卡片组件
interface ChartCardProps {
  title: string
  children: React.ReactNode
}

function ChartCard({ title, children }: ChartCardProps) {
  return (
    <div className="bg-white p-6 rounded-lg shadow-sm">
      <h2 className="text-lg font-semibold text-gray-900 mb-4">{title}</h2>
      {children}
    </div>
  )
}

// 数据表格组件
interface DataTableProps {
  data: typeof salesData
}

function DataTable({ data }: DataTableProps) {
  return (
    <div className="bg-white rounded-lg shadow-sm overflow-hidden">
      <div className="px-6 py-4 border-b border-gray-200">
        <h2 className="text-lg font-semibold text-gray-900">详细数据</h2>
      </div>
      <div className="overflow-x-auto">
        <table className="w-full">
          <thead className="bg-gray-50">
            <tr>
              <th className="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase">月份</th>
              <th className="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase">收入</th>
              <th className="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase">成本</th>
              <th className="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase">利润</th>
              <th className="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase">利润率</th>
            </tr>
          </thead>
          <tbody className="divide-y divide-gray-200">
            {data.map((row) => {
              const profit = row.revenue - row.cost
              const margin = ((profit / row.revenue) * 100).toFixed(1)
              return (
                <tr key={row.month} className="hover:bg-gray-50">
                  <td className="px-6 py-4 text-sm text-gray-900">{row.month}</td>
                  <td className="px-6 py-4 text-sm text-gray-900">¥{row.revenue.toLocaleString()}</td>
                  <td className="px-6 py-4 text-sm text-gray-900">¥{row.cost.toLocaleString()}</td>
                  <td className="px-6 py-4 text-sm text-green-600 font-medium">¥{profit.toLocaleString()}</td>
                  <td className="px-6 py-4 text-sm text-gray-900">{margin}%</td>
                </tr>
              )
            })}
          </tbody>
        </table>
      </div>
    </div>
  )
}

export default App
```

---

### Step 4: 添加简单的测试

创建文件：`tests/basic.spec.ts`

```typescript
import { test, expect } from '@playwright/test'

test('仪表板加载正常', async ({ page }) => {
  await page.goto('http://localhost:5173')
  
  // 检查标题
  await expect(page.locator('h1')).toContainText('数据仪表板')
  
  // 检查统计卡片
  await expect(page.locator('text=总收入')).toBeVisible()
  
  // 检查图表
  await expect(page.locator('text=收入与成本趋势')).toBeVisible()
})

test('数据表格显示正确', async ({ page }) => {
  await page.goto('http://localhost:5173')
  
  // 检查表格
  await expect(page.locator('table')).toBeVisible()
  
  // 检查有6行数据
  const rows = await page.locator('tbody tr').count()
  expect(rows).toBe(6)
})
```

配置Playwright，编辑 `playwright.config.ts`:

```typescript
import { defineConfig } from '@playwright/test'

export default defineConfig({
  testDir: './tests',
  use: {
    baseURL: 'http://localhost:5173',
  },
  webServer: {
    command: 'npm run dev',
    port: 5173,
    reuseExistingServer: true,
  },
})
```

安装Playwright:

```bash
npm install -D @playwright/test
npx playwright install
```

---

## 🚀 运行项目

### 启动开发服务器

```bash
npm run dev
```

打开浏览器访问：`http://localhost:5173`

### 运行测试

```bash
npx playwright test
```

---

## 📂 完整项目结构

```
data-dashboard/
├── README.md
├── SAMPLE.md (本文件)
├── package.json
├── tsconfig.json
├── vite.config.ts
├── tailwind.config.js
├── playwright.config.ts
├── index.html
├── src/
│   ├── main.tsx
│   ├── App.tsx ✅
│   ├── index.css ✅
│   └── vite-env.d.ts
└── tests/
    └── basic.spec.ts ✅
```

---

## 💡 在Cursor中扩展

### 让Claude帮你添加功能

选中App.tsx，按Ctrl+K：

```markdown
"请帮我改进这个仪表板：
1. 添加数据筛选功能（日期范围选择器）
2. 添加更多图表类型（饼图、面积图）
3. 实现数据导出功能
4. 添加响应式设计优化
5. 实现深色模式"
```

### 添加shadcn/ui组件

```bash
npx shadcn-ui@latest init
npx shadcn-ui@latest add button card select
```

然后让Claude帮你重构代码使用这些组件。

---

## ✅ 检查清单

- [ ] 成功创建了Vite项目
- [ ] 配置了Tailwind CSS
- [ ] 仪表板可以正常显示
- [ ] 图表渲染正确
- [ ] 数据表格工作正常
- [ ] Playwright测试通过

---

## 🎨 下一步

1. **连接真实API**：替换模拟数据
2. **添加更多图表**：饼图、雷达图等
3. **实现筛选功能**：日期、类别筛选
4. **优化性能**：使用React.memo
5. **添加动画**：Framer Motion
6. **部署上线**：Vercel或Netlify

---

**开始构建你的数据仪表板吧！📊**

