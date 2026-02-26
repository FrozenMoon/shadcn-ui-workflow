# shadcn/ui 主题定制模板

## CSS 变量结构

shadcn/ui 使用 CSS 变量实现主题系统，定义在 `:root`（亮色）和 `.dark`（暗色）中。

### 核心变量

```css
@layer base {
  :root {
    /* 背景和前景 */
    --background: 0 0% 100%;        /* 页面背景 */
    --foreground: 0 0% 3.9%;        /* 默认文字 */

    /* 卡片 */
    --card: 0 0% 100%;
    --card-foreground: 0 0% 3.9%;

    /* 弹窗 */
    --popover: 0 0% 100%;
    --popover-foreground: 0 0% 3.9%;

    /* 主色调 - 品牌色 */
    --primary: 0 0% 9%;             /* 主按钮、重要操作 */
    --primary-foreground: 0 0% 98%;

    /* 次要色 */
    --secondary: 0 0% 96.1%;        /* 次要按钮 */
    --secondary-foreground: 0 0% 9%;

    /* 静音色 */
    --muted: 0 0% 96.1%;            /* 禁用、辅助信息 */
    --muted-foreground: 0 0% 45.1%;

    /* 强调色 */
    --accent: 0 0% 96.1%;           /* 悬停、选中状态 */
    --accent-foreground: 0 0% 9%;

    /* 功能色 */
    --destructive: 0 84.2% 60.2%;   /* 删除、危险操作 */
    --destructive-foreground: 0 0% 98%;

    /* 边框和输入框 */
    --border: 0 0% 89.8%;
    --input: 0 0% 89.8%;
    --ring: 0 0% 3.9%;              /* focus ring */

    /* 圆角 */
    --radius: 0.5rem;
  }

  .dark {
    --background: 0 0% 3.9%;
    --foreground: 0 0% 98%;
    /* ... 暗色主题变量 */
  }
}
```

### 颜色格式说明

shadcn 使用 HSL 格式（不带 `hsl()`），便于 Tailwind 处理透明度：
- `--primary: 222 47% 11%` → 深蓝色
- 使用时：`bg-primary` 或 `bg-primary/50`（50% 透明度）

---

## 品牌色定制示例

### 示例 1：科技蓝主题

```css
:root {
  --primary: 217 91% 60%;           /* #3B82F6 蓝色 */
  --primary-foreground: 0 0% 100%;

  --accent: 217 91% 95%;            /* 浅蓝背景 */
  --accent-foreground: 217 91% 30%;
}

.dark {
  --primary: 217 91% 60%;
  --primary-foreground: 0 0% 100%;

  --accent: 217 91% 20%;
  --accent-foreground: 217 91% 90%;
}
```

### 示例 2：品牌绿主题

```css
:root {
  --primary: 142 76% 36%;           /* #22C55E 绿色 */
  --primary-foreground: 0 0% 100%;
}
```

### 示例 3：优雅紫主题

```css
:root {
  --primary: 262 83% 58%;           /* #8B5CF6 紫色 */
  --primary-foreground: 0 0% 100%;
}
```

---

## 圆角定制

```css
:root {
  /* 默认 - 中等圆角 */
  --radius: 0.5rem;    /* 8px */

  /* 更圆润 */
  --radius: 0.75rem;   /* 12px */

  /* 更方正 */
  --radius: 0.25rem;   /* 4px */
}
```

Tailwind 中的使用：
- `rounded-sm` → `calc(var(--radius) - 4px)`
- `rounded-md` → `calc(var(--radius) - 2px)`
- `rounded-lg` → `var(--radius)`
- `rounded-xl` → `calc(var(--radius) + 4px)`

---

## 扩展功能色

如果项目需要额外的语义色（如 success、warning），可以扩展：

```css
:root {
  /* 成功色 */
  --success: 142 76% 36%;
  --success-foreground: 0 0% 100%;

  /* 警告色 */
  --warning: 38 92% 50%;
  --warning-foreground: 0 0% 0%;

  /* 信息色 */
  --info: 199 89% 48%;
  --info-foreground: 0 0% 100%;
}
```

同时更新 `tailwind.config.js`：

```js
module.exports = {
  theme: {
    extend: {
      colors: {
        success: "hsl(var(--success))",
        warning: "hsl(var(--warning))",
        info: "hsl(var(--info))",
      },
    },
  },
}
```

---

## 主题预览页面结构

生成一个预览页面，包含：

1. **颜色展示**
   - Primary / Secondary / Accent / Destructive
   - Background / Foreground / Muted
   - 明暗主题对比

2. **组件预览**
   - Button（所有变体）
   - Input / Select
   - Card
   - Dialog

3. **主题切换器**
   - 明暗模式切换按钮

```tsx
// 示例结构
export default function ThemePreview() {
  return (
    <div className="container py-10">
      <h1>主题预览</h1>

      {/* 颜色展示 */}
      <section>
        <h2>颜色系统</h2>
        <div className="grid grid-cols-4 gap-4">
          <div className="bg-primary text-primary-foreground p-4 rounded-lg">
            Primary
          </div>
          {/* ... 其他颜色 */}
        </div>
      </section>

      {/* 组件预览 */}
      <section>
        <h2>按钮</h2>
        <div className="flex gap-2">
          <Button>Default</Button>
          <Button variant="secondary">Secondary</Button>
          <Button variant="outline">Outline</Button>
          <Button variant="ghost">Ghost</Button>
          <Button variant="destructive">Destructive</Button>
        </div>
      </section>

      {/* ... 更多组件 */}
    </div>
  )
}
```

---

## 定制流程

1. **收集品牌信息**
   - 品牌主色（色值或描述）
   - Logo 颜色参考
   - 风格偏好

2. **生成 CSS 变量**
   - 根据主色生成配套色系
   - 确保对比度符合 WCAG 标准

3. **更新配置文件**
   - `globals.css` / `index.css`
   - `tailwind.config.js`（如需扩展）

4. **生成预览页面**
   - 展示所有颜色和组件
   - 支持明暗主题切换

5. **与用户确认**
   - 迭代调整直到满意
