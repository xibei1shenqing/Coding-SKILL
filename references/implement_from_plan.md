# 根据方案文件实现代码 Reference

## 适用场景

当用户明确要求根据已经生成的方案文件进行代码实现、开始开发、实现方案、按方案修改项目时，使用本 reference。

典型场景：

- 用户说“根据这个方案写代码”
- 用户说“开始实现”
- 用户说“按 `docs/ai-plans/...` 实现”
- 用户说“按照刚才生成的方案改代码”
- 用户说“把这个方案落地到项目里”
- 用户说“根据方案文件实现功能 / 修 Bug / 重构 / 创建项目”

本 reference 只负责 **根据已确认的方案文件进行代码实现**，不负责重新设计方案。

------

## 核心目标

本 reference 的目标是：

1. 确认本次代码实现基于哪个方案文件
2. 读取该方案文件
3. 读取方案涉及的项目代码
4. 根据方案文件修改代码
5. 创建或更新实现报告文件
6. 在实现报告中记录“方案内容对应实现到了哪些代码位置”

实现阶段的核心产物不是新的方案，而是：

```
代码修改
+
实现报告文件
```

实现报告用于后续审查、继续修改代码、检查方案是否已经落地。

------

## 重要限制

使用本 reference 时，AI 不允许重新设计方案。

禁止行为：

- 不要重新定义需求
- 不要重新判断方案是否合理
- 不要重新生成方案文件
- 不要修改原方案文件
- 不要扩展方案外功能
- 不要把实现阶段变成二次方案设计
- 不要因为方案不够详细就停止实现并重新规划
- 不要顺手重构无关代码
- 不要引入方案外依赖
- 不要修改方案未涉及的业务行为
- 不要在未确认方案文件的情况下写代码
- 不要只在聊天中输出实现总结而不创建实现报告文件

如果实现过程中遇到方案没有写明的底层实现细节，应优先参考项目已有代码风格、框架约定和相邻模块实现方式完成代码，并在实现报告中记录：

```
方案未指定该底层细节，本次按项目现有风格实现。
```

------

## 与方案类 reference 的边界

方案类 reference 负责创建方案文件：

- `references/new_project_plan.md`
- `references/old_project_add_feature.md`
- `references/old_project_modify_feature.md`
- `references/old_project_fix_bug.md`
- `references/old_project_refactor_optimize.md`

本 reference 负责根据方案文件写代码。

关系如下：

```
方案类 reference
↓
生成 docs/ai-plans/.../*.md
↓
用户确认开始实现
↓
implement_from_plan.md
↓
修改代码
↓
生成 docs/ai-implementation/.../*.md
```

如果用户还没有方案文件，应先转入对应方案类 reference，而不是直接写代码。

------

## 工作流程

### 第一步：确认方案文件

当用户要求写代码时，必须先确认本次代码实现基于哪个方案文件。

如果用户已经明确提供方案文件路径，例如：

```
docs/ai-plans/add-feature/export_excel.md
```

则直接读取该方案文件。

如果用户没有提供方案文件路径，不允许直接写代码。

应先回复：

```
在开始写代码前，我需要先确认本次要根据哪个方案文件实现。

请提供方案文件路径，例如：

docs/ai-plans/add-feature/export_excel.md
docs/ai-plans/modify-feature/multi_course_task.md
docs/ai-plans/fix-bug/runtime/startup_error.md
docs/ai-plans/refactor-optimize/task_service.md
```

如果项目中可以读取 `docs/ai-plans/`，可以列出候选方案文件，但仍需用户确认使用哪一个。

在方案文件未确认前，不要修改代码。

------

### 第二步：读取方案文件

确认方案文件后，必须读取该方案文件。

读取后需要明确：

- 方案类型
- 方案目标
- 文件变更计划
- 数据结构要求
- 函数 / 模块 / 接口要求
- 测试和验收要求
- 风险和注意事项

注意：

本阶段只读取和理解方案，不重新审查方案是否合理。

------

### 第三步：读取项目代码

根据方案文件中的文件变更计划和实现目标，读取相关项目代码。

优先读取：

- 方案中明确提到的文件
- 方案中明确提到的类、函数、接口、模块
- 相关入口文件
- 相关配置文件
- 相关数据模型
- 相关测试文件
- 相关已有相似实现

如果方案涉及新增文件，应先确认项目现有目录结构和命名风格。

如果方案涉及依赖、配置、环境变量、路由、权限、中间件、任务队列、缓存或第三方服务，应读取相关配置文件。

------

### 第四步：根据方案实现代码

实现时必须遵守：

- 以方案文件为准
- 只实现方案文件中要求的内容
- 保持最小修改
- 优先复用项目已有代码风格
- 优先复用项目已有依赖
- 不实现方案外功能
- 不修改无关模块
- 不进行无关重构
- 不改变方案指定的接口、数据结构、状态流转或行为边界

如果方案中没有写明某个底层实现细节，但实现该方案必须补充，应按照以下优先级处理：

1. 项目已有相似实现
2. 当前模块已有代码风格
3. 框架常规约定
4. 最小可用实现

并在实现报告中记录该实现选择。

------

### 第五步：创建或更新实现报告文件

代码实现完成后，必须根据 `assets/implementation_report_template.md` 创建或更新实现报告文件。

实现报告路径根据方案文件路径自动映射。

映射规则：

```
docs/ai-plans/<plan_type>/<task_name>.md
↓
docs/ai-implementation/<plan_type>/<task_name>.md
```

示例：

```
docs/ai-plans/new-project/task_platform.md
↓
docs/ai-implementation/new-project/task_platform.md
```

```
docs/ai-plans/add-feature/export_excel.md
↓
docs/ai-implementation/add-feature/export_excel.md
```

```
docs/ai-plans/modify-feature/multi_course_task.md
↓
docs/ai-implementation/modify-feature/multi_course_task.md
```

```
docs/ai-plans/fix-bug/runtime/startup_error.md
↓
docs/ai-implementation/fix-bug/runtime/startup_error.md
```

```
docs/ai-plans/fix-bug/behavior/task_status_wrong.md
↓
docs/ai-implementation/fix-bug/behavior/task_status_wrong.md
```

```
docs/ai-plans/refactor-optimize/task_service.md
↓
docs/ai-implementation/refactor-optimize/task_service.md
```

如果 `docs/ai-implementation/` 或对应子目录不存在，应创建。

实现报告必须重点记录：

- 使用的方案文件
- 已完成的方案内容
- 方案章节与代码位置的对应关系
- 新增文件
- 修改文件
- 删除文件
- 新增或修改的函数 / 类 / 接口 / 命令
- 未完成内容
- 方案外的必要实现细节
- 验证记录

------

### 第六步：聊天中只输出简要结果

实现完成后，聊天中不要重复输出完整实现报告。

只需要简要说明：

```
已根据方案文件完成代码实现：

方案文件：
docs/ai-plans/<plan_type>/<task_name>.md

实现报告：
docs/ai-implementation/<plan_type>/<task_name>.md

本次主要完成：

1. ...
2. ...
3. ...

后续如需审查或继续修改代码，请优先查看实现报告文件。
```

------

## 实现报告修改规则

如果用户后续指出实现有问题，AI 不应只在聊天中解释。

AI 必须：

1. 读取原方案文件
2. 读取已有实现报告文件
3. 读取相关代码
4. 修改代码
5. 更新实现报告文件
6. 在聊天中只说明修改了哪些代码位置和实现报告章节

回复格式：

```
已根据反馈修改代码，并更新实现报告：

实现报告：
docs/ai-implementation/<plan_type>/<task_name>.md

本次修改：

1. 修改了代码位置：
   - 文件：
   - 函数 / 类 / 接口：
   - 修改原因：

2. 更新了实现报告章节：
   - 第 X 节：
   - 更新内容：
```

------

## 完成标准

本 reference 的任务完成标准是：

1. 已确认方案文件路径
2. 已读取方案文件
3. 已读取相关项目代码
4. 已根据方案文件完成代码修改
5. 已创建或更新实现报告文件
6. 实现报告中已记录方案内容与代码位置的对应关系
7. 已说明新增、修改、删除的文件
8. 已说明新增或修改的函数 / 类 / 接口 / 命令
9. 已记录未完成内容，如果存在
10. 已记录验证结果，如果执行了验证
11. 没有重新设计方案
12. 没有修改原方案文件
13. 没有只在聊天中输出实现总结