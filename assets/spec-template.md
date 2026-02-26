# [项目名称] UI 业务规范

> **版本**: 1.0
> **基础**: shadcn/ui
> **最后更新**: [日期]

本项目使用 shadcn/ui 作为基础组件库，本规范定义品牌定制和业务层面的 UI 约定。

---

## 1. 主题配置

### 1.1 品牌色

| 角色 | CSS 变量 | 色值 | 使用场景 |
|------|---------|------|----------|
| Primary | `--primary` | `hsl(...)` | 主按钮、重要链接、品牌强调 |
| Secondary | `--secondary` | `hsl(...)` | 次要按钮、辅助操作 |
| Accent | `--accent` | `hsl(...)` | 悬停状态、选中态 |
| Destructive | `--destructive` | `hsl(...)` | 删除、危险操作 |

### 1.2 扩展色（如有）

| 角色 | CSS 变量 | 使用场景 |
|------|---------|----------|
| Success | `--success` | 成功状态、正向反馈 |
| Warning | `--warning` | 警告状态、需要注意 |
| Info | `--info` | 提示信息、普通通知 |

### 1.3 圆角

项目使用 `--radius: [值]`，组件圆角层级：
- 小组件（Badge）：`rounded-sm`
- 中组件（Button、Input）：`rounded-md`
- 大组件（Card、Dialog）：`rounded-lg`

---

## 2. 组件使用规范

### 2.1 必须使用 shadcn 组件

以下场景**必须**使用 shadcn/ui 组件，禁止自己实现：

| 场景 | 使用组件 | 导入路径 |
|------|---------|----------|
| 按钮 | `Button` | `@/components/ui/button` |
| 输入框 | `Input` | `@/components/ui/input` |
| 下拉选择 | `Select` | `@/components/ui/select` |
| 复选框 | `Checkbox` | `@/components/ui/checkbox` |
| 开关 | `Switch` | `@/components/ui/switch` |
| 弹窗 | `Dialog` | `@/components/ui/dialog` |
| 确认框 | `AlertDialog` | `@/components/ui/alert-dialog` |
| 侧边栏 | `Sheet` | `@/components/ui/sheet` |
| 轻提示 | `toast` (Sonner) | `@/components/ui/sonner` |
| 卡片 | `Card` | `@/components/ui/card` |
| 表格 | `Table` | `@/components/ui/table` |
| 标签页 | `Tabs` | `@/components/ui/tabs` |

### 2.2 组件选择指南

**弹窗类型选择**：
- 表单编辑、详情查看 → `Dialog`
- 危险操作确认（删除） → `AlertDialog`
- 长表单、筛选面板 → `Sheet`（侧边滑出）
- 移动端操作菜单 → `Drawer`（底部滑出）

**提示类型选择**：
- 操作成功/失败反馈 → `toast`（自动消失）
- 需要用户阅读的重要信息 → `Alert`（页面内）
- 字段说明、图标解释 → `Tooltip`（悬停显示）

**按钮变体选择**：
- 主要操作（提交、确认） → `variant="default"`
- 次要操作（取消、返回） → `variant="secondary"` 或 `variant="outline"`
- 危险操作（删除） → `variant="destructive"`
- 轻量操作（更多、展开） → `variant="ghost"`

---

## 3. 业务组件规范

### 3.1 已定义的业务组件

| 组件名 | 用途 | 文件位置 |
|--------|------|----------|
| [组件名] | [用途] | `@/components/[path]` |

### 3.2 业务组件开发规范

创建新业务组件时：
1. **优先组合 shadcn 组件**，而非从零实现
2. 放置在 `@/components/` 下（非 `@/components/ui/`）
3. 遵循 shadcn 的 Props 命名风格
4. 支持 `className` 透传

```tsx
// 示例：业务组件封装
import { Card, CardContent, CardHeader, CardTitle } from "@/components/ui/card"
import { Avatar, AvatarFallback, AvatarImage } from "@/components/ui/avatar"

interface UserCardProps {
  user: { name: string; avatar: string; role: string }
  className?: string
}

export function UserCard({ user, className }: UserCardProps) {
  return (
    <Card className={className}>
      <CardHeader>
        <Avatar>
          <AvatarImage src={user.avatar} />
          <AvatarFallback>{user.name[0]}</AvatarFallback>
        </Avatar>
        <CardTitle>{user.name}</CardTitle>
      </CardHeader>
      <CardContent>
        <p className="text-muted-foreground">{user.role}</p>
      </CardContent>
    </Card>
  )
}
```

---

## 4. 布局规范

### 4.1 页面结构

```
┌─────────────────────────────────────┐
│  Header (h-14 / h-16)               │
├─────────────────────────────────────┤
│                                     │
│  Main Content                       │
│  (container mx-auto px-4)           │
│                                     │
├─────────────────────────────────────┤
│  Footer (可选)                      │
└─────────────────────────────────────┘
```

### 4.2 间距规范

| 场景 | 间距 | Tailwind 类 |
|------|------|-------------|
| 组件内元素 | 8-12px | `gap-2`, `gap-3` |
| 表单字段间 | 16-24px | `space-y-4`, `space-y-6` |
| 卡片内边距 | 16-24px | `p-4`, `p-6` |
| 模块间距 | 32-48px | `py-8`, `py-12` |
| 页面边距 | 16-24px | `px-4`, `px-6` |

### 4.3 响应式断点

遵循 Tailwind 默认断点：
- `sm`: 640px（大手机）
- `md`: 768px（平板）
- `lg`: 1024px（笔记本）
- `xl`: 1280px（桌面）

---

## 5. 禁止事项

### 5.1 样式相关

- ❌ 硬编码颜色值（如 `text-blue-500`），应使用主题变量（`text-primary`）
- ❌ 硬编码 px 值，应使用 Tailwind spacing
- ❌ 直接修改 `@/components/ui/` 下的组件源码

### 5.2 组件相关

- ❌ shadcn 已有组件时自己实现（如自定义 Modal）
- ❌ 在业务代码中直接使用 Radix 原语（应通过 shadcn 组件）
- ❌ 不遵循组件选择指南（如用 Dialog 做确认框）

---

## 6. 安装新组件

需要新 shadcn 组件时：

```bash
npx shadcn@latest add [component-name]
```

常用组件安装：
```bash
npx shadcn@latest add button input select checkbox switch
npx shadcn@latest add dialog alert-dialog sheet
npx shadcn@latest add card table tabs
npx shadcn@latest add toast sonner
npx shadcn@latest add form
```

---

## 7. 更新日志

### v1.0 ([日期])
- 初始版本
- 基于 shadcn/ui 主题定制
- 定义组件使用规范和业务组件规范
