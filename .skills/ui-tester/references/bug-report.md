# Bug Report Format

When a bug is confirmed (Layer 2 retry failed), output a bug block in the test report:

```markdown
## Bug #[N]: [简短标题]

**Severity**: critical | high | medium | low
**Module**: [module frontmatter value]
**Case**: [case name]
**Page**: [full URL at time of failure]
**Run ID**: [run_id]

### Steps to Reproduce
1. 导航到 [URL]
2. [操作步骤...]

### Expected
[assert 中描述的预期状态]

### Actual
[实际观察到的状态]

### Evidence
- Screenshot: ![failure screenshot](relative/path.png)
- Console errors: [paste if any]
```

## Severity Guide

| Severity | Condition |
|----------|-----------|
| `critical` | Page crash, white screen, cannot load |
| `high` | Core flow broken — cannot submit/save/create |
| `medium` | Feature broken but workaround exists |
| `low` | UI display issue (layout, text, style) |

## Flaky Report Block

Add at end of report for any cases with `flaky` status:

```markdown
## ⚠️ Flaky Report

| 用例 | 名称 | Flaky 次数 | 状态 |
|------|------|-----------|------|
| bkn/create | 创建业务知识网络 | 1 | 观察中 |
```
