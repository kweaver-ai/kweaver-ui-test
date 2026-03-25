# Report Format Reference

## Directory Structure Per Run

```
reports/
├── runs/
│   └── {run_id}/                  # 每次执行独立目录，自成一体
│       ├── report.md              # 人可读报告（提交 git 作为测试证据）
│       ├── report.json            # 机器可读记录（提交 git）
│       └── screenshots/           # 本次运行所有截图（与报告同目录）
│           ├── auth-login-001-step_start.png
│           ├── bkn-create-002-form_filled.png
│           └── bkn-create-003-after_success.png
└── latest.md                      # 最新报告副本（便于快速查阅）
```

Run ID 格式：`YYYY-MM-DD-HHmm`（本地时间）

截图路径（相对于 run 目录）：`screenshots/{module}-{case}-{index:03d}-{label}.png`

报告中截图引用使用相对路径，例：`![截图](screenshots/bkn-create-002-form_filled.png)`

## JSON Schema

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
      "status": "pass | fail | flaky | skip",
      "duration_ms": 4231,
      "screenshots": [
        "screenshots/bkn-create-001-step_start.png",
        "screenshots/bkn-create-002-form_filled.png"
      ]
    }
  ],
  "bugs": [],
  "flaky": []
}
```

## Markdown Report Structure

```markdown
# 测试报告 — {run_id}

## 概览
| 环境 | 开始 | 结束 | 通过率 |
|------|------|------|------|
| staging | 10:41 | 10:52 | 83% (10/12) |

## ✅ 通过用例（10）
| 用例 | 名称 | 耗时 |
|------|------|------|
| bkn/create | 创建业务知识网络 | 4.2s |

## ❌ 失败用例（1）
[Bug report blocks — see references/bug-report.md]

## ⚠️ Flaky 用例（1）
[Flaky report block — see references/bug-report.md]
```
