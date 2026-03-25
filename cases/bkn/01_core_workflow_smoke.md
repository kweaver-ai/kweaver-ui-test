---
module: bkn
name: 业务知识网络核心流程（冒烟）
tags: [smoke, e2e]
priority: p0
requires_session: true
screenshots:
  - bkn_created
  - object_created
  - teardown_done
preconditions:
  - 系统已部署且可访问
---

## 步骤

### 阶段一：创建业务知识网络
- 导航到业务知识网络列表页 `{{BASE_URL}}/vega/ontology`
- 等待页面加载完成，直到出现「新建」相关文字
- 点击带有「新建」文字的按钮
  - assert: 出现创建表单或弹窗
- 在 label 或 placeholder 包含「名称」的输入框中输入 `冒烟测试网络_自动化`
- 在「备注」相关输入框中输入 `由自动化测试创建`
- 点击带有「确定」或「保存」文字的按钮 
- 等待操作完成 <!-- screenshot: bkn_created -->
  - assert: 出现包含「成功」相关文字的提示
  - assert: 列表中出现 `冒烟测试网络_自动化` 的条目

### 阶段二：搜索验证与进入详情
- 导航到业务知识网络列表页 `{{BASE_URL}}/vega/ontology`
- 在包含「搜索」语义的输入框中输入 `冒烟测试网络_自动化`
- 按下回车键或点击搜索图标
  - assert: 列表区域呈现搜索结果
- 点击列表中名称严格为 `冒烟测试网络_自动化` 的名称链接或对应操作，进入详情页
  - assert: 页面发生跳转，标题或导航区显示当前所在网络名称为 `冒烟测试网络_自动化`

### 阶段三：清理数据 (Teardown)
- 导航回到业务知识网络列表页 `{{BASE_URL}}/vega/ontology`
- 找到名称为 `冒烟测试网络_自动化` 的整行数据，点击该行右侧带有「删除」语义的按钮或图标
  - assert: 出现二次确认删除的弹窗
- 点击二次确认弹窗中带有「确定」或「删除」文字的按钮 
- 等待操作完成 <!-- screenshot: teardown_done -->
  - assert: 出现包含「删除成功」相关文字的提示
  - assert: 列表中不再存在 `冒烟测试网络_自动化`

## 最终验证

- 无 JS 控制台 error
- 数据清理干净，未遗留脏数据
