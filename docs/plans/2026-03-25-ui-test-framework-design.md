# kweaver-ui-test 框架设计文档

> 日期：2026-03-25  
> 状态：已确认，待实施

---

## 一、概述

本项目由 OpenClaw 拉取并驱动，专注于 UI 自动化回归测试。核心理念：
- 用例以人可读的 Markdown 自然语言编写
- Agent-browser 负责浏览器执行，OpenClaw Agent 负责理解和调度
- 测试结果以双格式（JSON + Markdown）存入 git，作为测试证据留档

---

## 二、用例结构设计

### 2.1 文件结构

```
cases/
├── auth/
│   └── login.md          # 基础登录流程（session seed）
├── {module}/
│   ├── create.md
│   ├── search.md
│   └── ...
```

### 2.2 用例 Frontmatter 规范

```yaml
---
module: bkn                          # 模块名称
name: 创建业务知识网络                  # 用例名称（中文）
tags: [smoke, regression, e2e]       # 标签：smoke / regression / e2e / wip / skip
priority: p0                         # 优先级：p0 / p1 / p2
requires_session: true               # 是否需要登录态（true 则注入已有 session）
screenshots:                         # 声明截图时机（语义标签）
  - step_start                       # 用例开始时（自动）
  - step_end                         # 用例结束时（自动）
  - after_submit                     # 自定义：提交后
  - on_failure                       # 失败时（自动，始终开启）
---
```

**内置截图时机（无需声明，始终有效）：**
| 时机 | 触发条件 |
|------|---------|
| `step_start` | 用例开始时 |
| `step_end` | 用例正常结束时 |
| `on_failure` | 任意步骤失败时 |

**自定义截图时机（在步骤中用语义锚点标注）：**
```markdown
10. 点击「确定」按钮
11. 等待操作成功提示出现 <!-- screenshot: after_submit -->
```

### 2.3 截图文件命名规范

```
screenshots/{run_id}/{module}-{case}-{index}-{label}.png
```

例：`screenshots/2026-03-25-1041/bkn-create-002-after_submit.png`

---

## 三、Session 管理（登录复用）

### 3.1 工作流

```
运行开始
  │
  ├─ 有 requires_session: true 的用例？
  │     │
  │     ├─ ~/.session/{env}.json 存在且有效？
  │     │     └─ 是 → 注入 cookies，跳过登录
  │     │
  │     └─ 无效/不存在 → 执行 auth/login.md → 保存 cookies
  │
  └─ requires_session: false → 直接执行
```

### 3.2 Session 有效性校验

注入 session 后，访问一个需鉴权的页面（如 Dashboard），检测是否跳转到登录页。若跳转，则重新登录并刷新 session 文件。

### 3.3 Session 文件路径

```
~/.kweaver-ui-test/session/{env}.json
```

内容示例：
```json
{
  "env": "staging",
  "saved_at": "2026-03-25T10:41:44+08:00",
  "cookies": [...]
}
```

---

## 四、Bug 确认机制

### 4.1 三层确认流程

```
步骤执行失败
  │
  Layer 1: 自动判定
  ├─ HTTP 4xx/5xx → 直接标记 bug
  ├─ console.error 出现 → 标记 warning
  └─ 元素超时未出现 → 进入 Layer 2
         │
         Layer 2: 重试确认
         ├─ 等待 2s → 重试一次
         ├─ 重试成功 → 标记 flaky（进入 Flaky 追踪）
         └─ 重试失败 → 截图 + DOM snapshot → 进入 Layer 3
                │
                Layer 3: 自动分级
                ├─ 页面崩溃/白屏 → severity: critical
                ├─ 核心流程断裂 → severity: high
                ├─ 功能异常但有 workaround → severity: medium
                └─ UI 展示问题 → severity: low
```

### 4.2 Bug 报告格式

```markdown
## Bug #[N]: [简短标题]

**Severity**: critical | high | medium | low
**Module**: [模块名]
**Case**: [用例名]
**Page**: [完整 URL]
**Run ID**: [run_id]

### Steps to Reproduce
1. 导航到 [URL]
2. 点击 [元素描述]
3. ...

### Expected
[应该发生什么]

### Actual
[实际发生了什么]

### Evidence
- Screenshot: ![截图](相对路径)
- Console errors: [如有]
- DOM Snapshot: (附件或折叠块)
```

### 4.3 Flaky 用例追踪

所有标记为 `flaky` 的用例，单独汇总到报告末尾的 **Flaky Report** 区块：

```markdown
## ⚠️ Flaky Report

| 用例 | Flaky 次数 | 最近出现 | 状态 |
|------|-----------|---------|------|
| bkn/create | 3 | 2026-03-25 | 观察中 |
```

---

## 五、报告规范

### 5.1 文件输出

每次运行生成两个文件：

```
reports/
├── runs/
│   └── {run_id}.json      # 机器可读，程序消费
├── {run_id}.md             # 人可读，测试证据
└── latest.md               # 始终 copy 最新报告（便于快速查阅）
```

Run ID 格式：`YYYY-MM-DD-HHmm`（本地时间）

### 5.2 JSON 运行记录结构

```json
{
  "run_id": "2026-03-25-1041",
  "env": "staging",
  "started_at": "ISO8601",
  "ended_at": "ISO8601",
  "summary": {
    "total": 12,
    "pass": 10,
    "fail": 1,
    "flaky": 1,
    "skip": 0
  },
  "cases": [
    {
      "id": "bkn/create",
      "name": "创建业务知识网络",
      "status": "pass",
      "duration_ms": 4231,
      "screenshots": ["screenshots/2026-03-25-1041/bkn-create-001-step_start.png"]
    }
  ],
  "bugs": [],
  "flaky": []
}
```

### 5.3 Markdown 报告结构

```
# 测试报告 — {run_id}

## 概览
- 环境：staging | 时间：... | 通过率：83%

## ✅ 通过用例（10）
| 用例 | 耗时 |
|------|------|
| bkn/create | 4.2s |

## ❌ 失败用例（1）
### [Bug 详情块]

## ⚠️ Flaky 用例（1）
### [Flaky Report 块]
```

---

## 六、agent-browser 执行规范（规范补充）

在现有 AGENTS.md 基础上新增以下规则：

1. **Session 注入优先** — 有 `requires_session: true` 的用例，先校验 session，再执行
2. **截图受控** — 仅在 frontmatter `screenshots:` 声明的时机及 `on_failure` 时截图，禁止随意截图
3. **失败重试一次** — 失败后等待 2s 重试，重试成功标记 flaky，重试失败才确认 Bug
4. **Bug 必须完整** — 每个 Bug 报告必须包含：severity / 复现步骤 / 截图 / 当前 URL
5. **双格式报告** — 每次运行结束必须生成 `.json` + `.md` 两个报告文件
6. **skip 标签** — frontmatter `tags` 含 `skip` 或 `wip` 的用例，直接跳过，不计入统计
