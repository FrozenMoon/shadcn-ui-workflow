# shadcn/ui Workflow Skill

针对使用 **shadcn/ui** 的项目的 UI 开发工作流 Skill for Claude Code。

## 安装

将此仓库克隆到项目的 `.claude/skills/` 目录：

```bash
git clone https://github.com/FrozenMoon/shadcn-ui-workflow.git YOUR_PROJECT/.claude/skills/shadcn-ui-workflow
```

或全局安装（所有项目可用）：

```bash
git clone https://github.com/FrozenMoon/shadcn-ui-workflow.git ~/.claude/skills/shadcn-ui-workflow
```

## 核心理念

shadcn/ui 已提供优秀的基础组件，本 Skill 聚焦于：
- **品牌定制**：基于 shadcn 的 CSS 变量系统定制品牌主题
- **业务规范**：定义组件选择、布局约定等业务层面规范
- **组件复用**：确保优先使用 shadcn 组件，避免重复造轮子

## 命令

### theme - 主题定制

基于 shadcn/ui 的 CSS 变量系统定制品牌色。

```bash
/shadcn-ui-workflow theme 使用深蓝色作为主色调，支持明暗主题
```

输出：
- 更新 `globals.css` 中的 CSS 变量
- 可选：扩展 `tailwind.config.js`
- 生成主题预览页面

### spec - 业务规范

生成业务层面的 UI 规范（非组件样式规范）。

```bash
/shadcn-ui-workflow spec
```

输出：
- `doc/UI业务规范.md`
- 更新 CLAUDE.md 添加规范引用

规范内容：
- 主题配置说明
- 组件选择指南（Dialog vs Sheet 等）
- 业务组件规范
- 布局和间距约定

### implement - 规范实现

优先使用 shadcn/ui 组件实现功能。

```bash
/shadcn-ui-workflow implement 实现用户设置页面
```

核心原则：
- 先检查 shadcn 是否有对应组件
- 优先使用 shadcn 组件
- 仅在 shadcn 没有时才自定义
- 自定义组件基于 shadcn 组件组合封装

### check - 规范检查

检查代码是否正确使用 shadcn/ui。

```bash
/shadcn-ui-workflow check src/pages/
```

检查项：
- shadcn 组件覆盖率
- 主题变量使用率
- 组件选择合理性

## shadcn 组件速查

| 需求 | shadcn 组件 |
|------|------------|
| 按钮 | Button |
| 输入框 | Input |
| 下拉选择 | Select |
| 弹窗 | Dialog |
| 确认框 | AlertDialog |
| 侧边抽屉 | Sheet |
| 轻提示 | Sonner / Toast |
| 卡片 | Card |
| 表格 | Table |
| 标签页 | Tabs |

完整列表见 [SKILL.md](SKILL.md)。

## 文件结构

```
shadcn-ui-workflow/
├── SKILL.md                    # 主文件
├── README.md                   # 本文件
├── LICENSE                     # MIT 协议
├── templates/
│   ├── theme-customization.md  # 主题定制模板
│   └── spec-template.md        # 规范文档模板
└── references/
    └── modes.md                # 各模式详细说明
```

## 协议

MIT License
