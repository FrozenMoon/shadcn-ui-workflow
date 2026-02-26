# 模式详细说明

各命令的详细执行流程。

---

## theme 模式

### 与用户确认的问题

1. **品牌主色**：
   - 提供具体色值（如 `#3B82F6`）
   - 或描述（如"科技蓝"、"品牌绿"）
   - 或参考（如"类似 Vercel 的风格"）

2. **明暗主题**：
   - 仅亮色主题
   - 明暗双主题（推荐）
   - 跟随系统

3. **圆角偏好**：
   - shadcn 默认（中等）
   - 更圆润（适合 C 端产品）
   - 更方正（适合专业工具）

### 输出文件

1. **CSS 变量**：更新 `globals.css` 或 `app/globals.css`
2. **Tailwind 配置**：如需扩展色，更新 `tailwind.config.js`
3. **预览页面**：`app/theme-preview/page.tsx` 或 `src/pages/ThemePreview.tsx`

### 完成提示

```
✅ 主题定制完成

已更新文件：
1. app/globals.css - CSS 变量
2. tailwind.config.js - 扩展配置（如有）
3. app/theme-preview/page.tsx - 预览页面

品牌色配置：
- Primary: hsl(217, 91%, 60%) - 科技蓝
- 支持明暗主题切换

预览：启动开发服务器后访问 /theme-preview

满意后使用 /shadcn-ui-workflow spec 生成业务规范。
```

---

## spec 模式

### 前提条件

- 已完成主题定制（theme 模式）
- 项目已安装 shadcn/ui

### 输出

1. **规范文档**：`doc/UI业务规范.md`
2. **项目集成**：更新 CLAUDE.md

### 规范文档内容

与通用 UI 规范不同，shadcn 项目的规范聚焦：
- 主题配置说明
- shadcn 组件使用规范
- 组件选择指南
- 业务组件规范
- 布局和间距约定

### CLAUDE.md 更新

```markdown
## UI 开发规范

本项目使用 shadcn/ui，开发 UI 时必须：
1. 优先使用 shadcn/ui 组件
2. 遵循 `doc/UI业务规范.md` 中的约定
3. 使用 `/shadcn-ui-workflow implement` 命令开发
```

---

## implement 模式

### 核心原则

**优先使用 shadcn/ui 组件，不要重复造轮子**

### 执行流程

```
1. 读取项目 UI 业务规范
   ↓
2. 分析用户需求
   ↓
3. 检查 shadcn 是否有对应组件
   ↓
4. 如有 → 使用 shadcn 组件实现
   如无 → 基于 shadcn 组件组合/封装
   ↓
5. 确保使用主题变量（非硬编码颜色）
   ↓
6. 输出实现报告
```

### 组件检查清单

在实现前，检查以下 shadcn 组件是否可用：

**表单相关**：
- 按钮 → `Button`
- 输入框 → `Input`
- 多行文本 → `Textarea`
- 下拉选择 → `Select`
- 复选框 → `Checkbox`
- 单选框 → `RadioGroup`
- 开关 → `Switch`
- 滑块 → `Slider`
- 日期选择 → `Calendar` + `Popover`
- 表单验证 → `Form` (react-hook-form)

**弹窗相关**：
- 通用弹窗 → `Dialog`
- 确认弹窗 → `AlertDialog`
- 侧边栏 → `Sheet`
- 底部抽屉 → `Drawer`
- 命令面板 → `Command`

**反馈相关**：
- 轻提示 → `Sonner` / `Toast`
- 页面警告 → `Alert`
- 工具提示 → `Tooltip`
- 进度条 → `Progress`
- 骨架屏 → `Skeleton`

**导航相关**：
- 标签页 → `Tabs`
- 导航菜单 → `NavigationMenu`
- 面包屑 → `Breadcrumb`
- 分页 → `Pagination`
- 侧边导航 → `Sidebar`

**数据展示**：
- 卡片 → `Card`
- 表格 → `Table` / `DataTable`
- 头像 → `Avatar`
- 徽章 → `Badge`
- 折叠面板 → `Accordion` / `Collapsible`

### 输出报告格式

```
✅ 实现完成

使用的 shadcn 组件：
- Dialog（弹窗容器）
- Form（表单验证）
- Input, Select, Button（表单元素）

需要安装的组件：
npx shadcn@latest add dialog form input select button

创建的业务组件：
- UserEditDialog（基于 Dialog + Form 封装）

文件变更：
- 新增：src/components/UserEditDialog.tsx
- 修改：src/pages/UserList.tsx
```

---

## check 模式

### 检查维度

1. **shadcn 组件覆盖率**
   - 是否有可用 shadcn 组件但没使用
   - 是否有自定义实现了 shadcn 已有的组件

2. **导入路径正确性**
   - shadcn 组件应从 `@/components/ui/` 导入
   - 业务组件应从 `@/components/` 导入

3. **主题变量使用**
   - 是否使用了 CSS 变量（`text-primary`）
   - 是否有硬编码颜色（`text-blue-500`）

4. **组件选择合理性**
   - Dialog vs AlertDialog vs Sheet 是否选择正确
   - Toast vs Alert 是否选择正确

### 输出报告

```markdown
## shadcn/ui 规范检查报告

检查范围：src/
检查时间：[时间]

### 1. shadcn 组件使用

✅ 正确使用的组件：
- Button (15 处)
- Dialog (3 处)
- Input (8 处)

⚠️ 建议使用 shadcn 替代：
- `src/components/Modal.tsx` → 使用 `Dialog`
- `src/components/Confirm.tsx` → 使用 `AlertDialog`
- `src/components/CustomSelect.tsx` → 使用 `Select`

### 2. 主题变量检查

⚠️ 发现硬编码颜色：
- `src/pages/Home.tsx:42` - `text-blue-500` → 应为 `text-primary`
- `src/components/Card.tsx:15` - `bg-gray-100` → 应为 `bg-muted`

### 3. 组件选择检查

⚠️ 组件选择建议：
- `src/pages/Settings.tsx:89` - 删除确认使用了 `Dialog`
  → 建议使用 `AlertDialog`（危险操作应使用确认弹窗）

### 总结

| 指标 | 数值 |
|------|------|
| shadcn 组件覆盖率 | 85% |
| 主题变量使用率 | 92% |
| 待优化项 | 5 处 |

建议优先处理：
1. 替换自定义 Modal 为 Dialog
2. 修复硬编码颜色
```
