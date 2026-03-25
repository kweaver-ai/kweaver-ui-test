# Reports

每次测试运行的所有产物（报告 + 截图）统一存放在对应 run 目录下，自成一体。

## 目录结构

```
reports/
├── runs/
│   └── {run_id}/                  # 每次执行独立目录
│       ├── report.md              # 人可读报告（测试证据，提交 git）
│       ├── report.json            # 机器可读记录（提交 git）
│       └── screenshots/           # 本次运行所有截图
│           ├── auth-login-001-step_start.png
│           └── bkn-create-002-after_success.png
└── latest.md                      # 最新报告副本（便于快速查阅）
```

Run ID 格式：`YYYY-MM-DD-HHmm`（本地时间）

详细格式规范见 [`.skills/ui-tester/references/report-format.md`](../.skills/ui-tester/references/report-format.md)
