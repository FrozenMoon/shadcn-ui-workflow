---
name: shadcn-ui-workflow
description: >
  MUST use when implementing UI with shadcn/ui in this project.
  Triggers: shadcn, radix, 实现页面, 开发UI, 写组件, button, form, modal, dialog,
  toast, card, input, 样式, 界面, implement page, create component.
  Ensures consistent use of shadcn/ui components and project theme.
---

# shadcn/ui Workflow

针对使用 shadcn/ui 的项目的 UI 开发工作流。

**核心理念**：shadcn/ui 已提供优秀的基础组件，本 skill 聚焦于：
- 品牌主题定制
- 业务组件规范
- 确保正确使用 shadcn 组件

## 命令速查

| 命令 | 用途 | 触发场景 |
|------|------|----------|
| `theme` | 定制品牌主题 | 项目初期定制 shadcn 主题 |
| `spec` | 生成业务规范 | 主题确认后 |
| `implement` | 按规范实现功能 | 日常开发 |
| `check` | 检查规范遵守 | PR 前检查 |

---

## 命令详情

### theme - 定制品牌主题

**目标**：基于 shadcn/ui 的主题系统，定制项目品牌色。

**输出**：
1. 定制的 CSS 变量（`globals.css` 或 `index.css`）
2. 可选：Tailwind 配置扩展
3. 主题预览页面

**流程**：
1. 确认品牌色（Primary、Secondary、Accent）
2. 生成明暗两套主题的 CSS 变量
3. 更新 `tailwind.config` 如需扩展
4. 生成主题预览页面

**与用户确认**：
- 品牌主色调（提供色值或描述如"科技蓝"）
- 是否需要明暗主题切换
- 圆角偏好（shadcn 默认 / 更圆润 / 更方正）

**详细模板**：见 [assets/theme-customization.md](assets/theme-customization.md)

---

### spec - 生成业务规范

**前提**：已完成主题定制

**输出**：
1. 规范文档：`doc/UI业务规范.md`
2. 项目集成：更新 CLAUDE.md

**规范内容**（与纯 Tailwind 项目不同）：
- ✅ 品牌色定义（CSS 变量映射）
- ✅ 组件选择指南（何时用 Dialog vs Sheet）
- ✅ 业务组件规范（如 UserCard、PricingTable）
- ✅ 布局和间距规范
- ❌ ~~基础组件样式~~（shadcn 已定义）

**详细模板**：见 [assets/spec-template.md](assets/spec-template.md)

---

### implement - 按规范实现功能

**核心原则**：**优先使用 shadcn/ui 组件，不要自己造轮子**

**执行流程**：
1. 读取项目规范
2. 检查 shadcn 是否有对应组件
3. 优先使用 shadcn 组件实现
4. 仅在 shadcn 没有时才自定义

**shadcn 组件检查清单**：
```
按钮/操作 → Button
表单输入 → Input, Textarea, Select, Checkbox, Radio, Switch
弹窗 → Dialog, AlertDialog, Sheet, Drawer
提示 → Toast (Sonner), Alert, Tooltip
导航 → Tabs, NavigationMenu, Breadcrumb
数据展示 → Card, Table, Avatar, Badge
布局 → Separator, ScrollArea, Collapsible
```

**输出格式**：
```
✅ 使用 shadcn 组件：Dialog, Button, Input
📦 需要安装：npx shadcn@latest add [component]
🔧 自定义组件：UserAvatar（基于 Avatar 封装）
```

---

### check - 检查规范遵守

**检查项**：

1. **shadcn 组件使用**
   - 是否有可用 shadcn 组件但没使用
   - 是否正确导入（从 `@/components/ui`）

2. **主题变量使用**
   - 是否使用了项目定义的 CSS 变量
   - 是否有硬编码颜色值

3. **业务规范遵守**
   - 组件选择是否符合规范
   - 布局间距是否一致

**输出格式**：
```
## shadcn/ui 规范检查报告

### shadcn 组件使用
✅ 正确使用：Button, Dialog, Input
⚠️ 建议使用 shadcn：
  - src/components/Modal.tsx → 使用 Dialog 替代
  - src/components/Notification.tsx → 使用 Toast 替代

### 主题变量
⚠️ 发现硬编码颜色：
  - src/pages/Home.tsx:42 → `text-blue-500` 应为 `text-primary`

### 总结
- shadcn 组件覆盖率：85%
- 主题变量使用率：90%
```

---

## shadcn/ui 组件速查

### 常用组件映射

| 需求 | shadcn 组件 | 安装命令 |
|------|------------|----------|
| 按钮 | Button | `npx shadcn@latest add button` |
| 输入框 | Input | `npx shadcn@latest add input` |
| 下拉选择 | Select | `npx shadcn@latest add select` |
| 复选框 | Checkbox | `npx shadcn@latest add checkbox` |
| 开关 | Switch | `npx shadcn@latest add switch` |
| 弹窗 | Dialog | `npx shadcn@latest add dialog` |
| 确认框 | AlertDialog | `npx shadcn@latest add alert-dialog` |
| 侧边抽屉 | Sheet | `npx shadcn@latest add sheet` |
| 轻提示 | Sonner | `npx shadcn@latest add sonner` |
| 卡片 | Card | `npx shadcn@latest add card` |
| 表格 | Table | `npx shadcn@latest add table` |
| 标签页 | Tabs | `npx shadcn@latest add tabs` |
| 工具提示 | Tooltip | `npx shadcn@latest add tooltip` |
| 下拉菜单 | DropdownMenu | `npx shadcn@latest add dropdown-menu` |
| 表单 | Form | `npx shadcn@latest add form` |

### 组件选择指南

**弹窗类**：
- `Dialog`：通用弹窗，表单、详情展示
- `AlertDialog`：需要确认的危险操作
- `Sheet`：侧边滑出，适合移动端或长表单
- `Drawer`：底部滑出，移动端常用

**提示类**：
- `Toast/Sonner`：操作反馈，自动消失
- `Alert`：页面内提示，需要用户注意
- `Tooltip`：悬停说明，简短文字

---

## 文件结构

```
shadcn-ui-workflow/
├── SKILL.md              # 本文件
├── README.md             # 使用说明
├── assets/
│   ├── theme-customization.md  # 主题定制模板
│   └── spec-template.md        # 规范文档模板
└── references/
    └── modes.md          # 各模式详细说明
```

---

## 快速决策

**用户说"定制主题"** → 用 `theme`
**用户说"生成规范"** → 用 `spec`
**用户说"实现功能"** → 用 `implement`（先检查 shadcn 有无组件）
**用户说"检查代码"** → 用 `check`
