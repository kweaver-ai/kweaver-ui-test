# kweaver-ui-test

基于 **OpenClaw** + **agent-browser** 的 UI 自动化回归测试用例仓库。

> 本仓库不包含测试执行代码。仅管理 **环境变量** 和 **自然语言测试用例**。
> 由 OpenClaw 拉取并通过 agent-browser 执行。

## 架构

```
本仓库 (Git)          OpenClaw (AI Agent)       agent-browser (CLI)
┌────────────┐        ┌──────────────┐          ┌───────────────┐
│ config/    │─pull──▶│ 读取用例      │─invoke──▶│ open / click  │
│ cases/     │        │ 理解步骤      │          │ fill / snap   │
│ openclaw/  │        │ 判断结果      │          │ screenshot    │
└────────────┘        └──────────────┘          └───────────────┘
```

## 目录结构

```
├── config/              # 多环境配置
│   ├── env.dev.json
│   ├── env.staging.json
│   └── env.production.json
├── cases/               # 自然语言测试用例
│   ├── auth/            # 认证模块
│   ├── dashboard/       # 仪表盘模块
│   └── knowledge/       # 知识库模块
├── openclaw/            # OpenClaw 智能体配置
├── screenshots/         # 截图输出
└── reports/             # 报告输出
```

## 用例格式

```markdown
---
module: auth
name: 用户登录流程
tags: [smoke, regression]
priority: p0
preconditions:
  - 系统已部署且可访问
---

## 步骤

1. 打开登录页面 `{{BASE_URL}}/login`
2. 在「用户名」输入框中输入 `{{TEST_USER}}`
3. 点击「登录」按钮

## 预期结果

- 页面跳转到仪表盘
```

## 标记系统

| 标记 | 含义 | 使用场景 |
|------|------|----------|
| `smoke` | 冒烟测试 | 部署后快速验证 |
| `regression` | 回归测试 | 变更后验证 |
| `e2e` | 端到端 | 完整业务流程 |
| `p0` / `p1` / `p2` | 优先级 | 按重要性分级 |
| `skip` | 跳过 | 暂时禁用 |
| `wip` | 进行中 | 未完成 |

## 环境切换

环境配置位于 `config/env.{name}.json`，通过 `{{BASE_URL}}` 等占位符在用例中引用。

## 配置说明

```bash
# 复制环境配置模板
cp .env.example .env
# 编辑 .env 填入实际 URL 和测试凭据
```
