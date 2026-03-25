---
module: auth
name: 用户登录（Session Seed）
tags: [smoke]
priority: p0
requires_session: false
screenshots:
  - after_login
preconditions:
  - 系统已部署且可访问
  - 环境变量 TEST_USER / TEST_PASSWORD 已配置
---

## 说明

此用例是所有需要登录态用例的 **Session Seed**。
执行后 agent-browser 通过 `AGENT_BROWSER_SESSION_NAME` 自动持久化登录态，
后续用例无需重新执行登录。

## 步骤

- 导航到登录页 `{{BASE_URL}}/login`
- 等待页面加载完成
  - assert: 页面出现用户名输入框
  - assert: 页面出现密码输入框
- 在用户名输入框中输入 `{{TEST_USER}}`
- 在密码输入框中输入 `{{TEST_PASSWORD}}`
- 点击「登录」按钮
- 等待页面跳转完成 <!-- screenshot: after_login -->
  - assert: 当前 URL 不包含 `/login`
  - assert: 页面出现用户名或用户头像，表明已登录

## 最终验证

- 无 JS 控制台 error
