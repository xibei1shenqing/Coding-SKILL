# Coding Skill

## 项目简介

本项目是一个用于辅助 AI 编写代码的 coding skill。

它的核心目标不是让 AI 一上来直接写代码，而是先判断用户的代码任务属于哪一类，再进入对应的流程，生成可审查、可修改、可追踪的方案文件。方案确认后，再根据方案文件进行代码实现，并生成实现报告。

本项目强调：

- 先分类，再处理
- 先方案，再实现
- 方案和代码实现分离
- 聊天上下文不承载长期方案内容
- 所有关键设计和实现结果都落到 Markdown 文件中

------

## 1. 使用场景

当用户希望 AI 辅助完成代码项目相关工作时，可以使用本 skill。

典型场景包括：

- 从 0 到 1 创建一个新项目
- 在已有项目中添加一个新功能
- 修改已有项目中的功能行为
- 修复已有项目中的 Bug
- 在功能行为不变的前提下重构或优化代码
- 根据已经生成的方案文件写代码
- 根据方案文件继续修改代码
- 审查某个方案是否已经落实到代码中

本 skill 适合处理的是“项目级”或“工程级”的代码任务，而不是简单的孤立代码片段。

------

## 2. 核心贡献

### 2.1 对写代码任务进行分类

本项目将代码任务分成两大类、五小类。

整体分类如下：

```
1. 新项目
   1.0 从 0 到 1 创建项目

2. 老项目
   2.1 添加功能
   2.2 修改功能
   2.3 修复 Bug
   2.4 重构优化
```

#### 1.0 新项目：从 0 到 1 创建项目

适用于当前没有完整项目，用户希望从零开始规划一个项目的情况。

重点关注：

- 项目目标
- 用户角色
- MVP 范围
- 核心业务闭环
- 技术栈
- 数据模型
- 模块划分
- 项目目录结构
- 第一版开发顺序
- 验收标准

输出方案文件：

```
docs/ai-plans/new-project/<project_name>.md
```

------

#### 2.1 老项目：添加功能

适用于已有项目中新增一个原本不存在的功能能力。

重点关注：

- 新增什么功能
- 功能边界
- 新增哪些文件
- 修改哪些旧文件
- 函数设计和调用链
- 数据结构
- 对旧功能的影响
- 测试和验收方案

输出方案文件：

```
docs/ai-plans/add-feature/<feature_name>.md
```

------

#### 2.2 老项目：修改功能

适用于已有功能已经存在，但用户希望改变它的行为、流程、接口、数据结构或状态流转。

重点关注：

- 旧功能现状
- 新功能目标行为
- 新旧行为差异
- 兼容策略
- 数据迁移策略
- 旧行为依赖点
- 回归测试

输出方案文件：

```
docs/ai-plans/modify-feature/<feature_name>.md
```

------

#### 2.3 老项目：修复 Bug

适用于已有项目中存在错误、异常或不符合预期的行为。

本项目将 Bug 分为两类：

```
运行类 Bug：
程序无法启动、接口报错、命令失败、测试失败、运行时异常等。

行为类 Bug：
程序能运行，但功能结果和预期不一致。
```

行为类 Bug 必须先确认“预期行为来源”，例如：

- 用户明确描述
- 已有方案文件
- 测试用例
- README / API 文档
- 注释、命名或代码上下文推断

输出方案文件：

```
docs/ai-plans/fix-bug/runtime/<bug_name>.md
docs/ai-plans/fix-bug/behavior/<bug_name>.md
```

------

#### 2.4 老项目：重构优化

适用于已有功能行为正确，但用户希望改善代码结构、可读性、可维护性、性能或模块边界。

重点关注：

- 当前行为基线
- 不允许改变的行为
- 当前代码问题
- 重构优化目标
- 行为等价验证
- 性能指标，如果涉及性能优化
- 风险和回滚思路

核心原则：

```
功能行为不变，只改善实现。
```

输出方案文件：

```
docs/ai-plans/refactor-optimize/<target_name>.md
```

------

### 2.2 根据方案写代码的策略

本项目的另一个核心贡献是提出“根据方案文件写代码”的策略。

也就是说，AI 不应该在聊天中边想边写代码，而应该遵循以下流程：

```
用户提出需求
↓
AI 判断任务类型
↓
AI 进入对应 reference
↓
AI 创建独立方案文件
↓
用户审查和修改方案文件
↓
用户确认开始实现
↓
AI 根据方案文件写代码
↓
AI 创建实现报告文件
```

代码实现阶段必须基于明确的方案文件。

如果用户只说“开始写代码”，但没有说明根据哪个方案文件实现，AI 必须先确认方案文件路径。

方案文件路径示例：

```
docs/ai-plans/add-feature/export_excel.md
docs/ai-plans/modify-feature/multi_course_task.md
docs/ai-plans/fix-bug/runtime/startup_error.md
docs/ai-plans/refactor-optimize/task_service.md
```

实现完成后，不只在聊天中输出总结，而是生成实现报告文件。

实现报告路径规则：

```
docs/ai-plans/<plan_type>/<task_name>.md
↓
docs/ai-implementation/<plan_type>/<task_name>.md
```

示例：

```
docs/ai-plans/add-feature/export_excel.md
↓
docs/ai-implementation/add-feature/export_excel.md
```

实现报告重点记录：

- 本次使用的方案文件
- 已完成方案中的哪些内容
- 方案章节对应到哪些代码位置
- 新增了哪些文件
- 修改了哪些文件
- 新增或修改了哪些函数、类、接口或命令
- 哪些内容未完成
- 是否有方案外的必要实现细节
- 执行了哪些验证

这样可以让后续审查和继续修改代码有明确依据。

------

## 3. 目录结构

推荐目录结构：

```
coding_skill/
  SKILL.md

  references/
    common_requirement_clarification.md

    new_project_plan.md

    old_project_add_feature.md
    old_project_modify_feature.md
    old_project_fix_bug.md
    old_project_refactor_optimize.md

    implement_from_plan.md

  assets/
    new_project_plan_template.md

    add_feature_plan_template.md
    modify_feature_plan_template.md

    fix_runtime_bug_template.md
    fix_behavior_bug_template.md

    refactor_optimize_plan_template.md

    implementation_report_template.md
```

------

## 4. 输出文件约定

### 方案文件

所有方案文件统一放在：

```
docs/ai-plans/
```

具体路径：

```
docs/ai-plans/new-project/<project_name>.md

docs/ai-plans/add-feature/<feature_name>.md

docs/ai-plans/modify-feature/<feature_name>.md

docs/ai-plans/fix-bug/runtime/<bug_name>.md
docs/ai-plans/fix-bug/behavior/<bug_name>.md

docs/ai-plans/refactor-optimize/<target_name>.md
```

### 实现报告文件

所有实现报告统一放在：

```
docs/ai-implementation/
```

路径和方案文件保持同名，只改变根目录：

```
docs/ai-plans/add-feature/export_excel.md
docs/ai-implementation/add-feature/export_excel.md
```

------

## 5. 设计原则

本项目遵循以下设计原则：

### 5.1 方案和实现分离

方案阶段只负责设计，不直接写代码。

实现阶段只负责根据方案文件写代码，不重新设计方案。

------

### 5.2 文件作为长期上下文

不要把完整方案长期堆在聊天上下文里。

方案和实现结果都应该写入项目中的 Markdown 文件：

```
docs/ai-plans/
docs/ai-implementation/
```

------

### 5.3 先确认需求，再生成方案

如果用户对新增功能、修改功能、Bug 现象或重构目标描述不明确，AI 必须先询问最小必要信息。

不要在需求不清楚时直接生成方案。

------

### 5.4 先确认方案文件，再写代码

如果用户要求写代码，但没有指定方案文件，AI 必须先确认本次要根据哪个方案文件实现。

不要在未确认方案文件的情况下直接写代码。

------

### 5.5 最小改动

无论是添加功能、修改功能、修复 Bug，还是重构优化，都应优先遵循最小改动原则：

- 不顺手重构无关代码
- 不引入未说明的新依赖
- 不扩大需求范围
- 不改变未要求改变的行为
- 不实现方案外功能

------

## 6. 总结

本项目的核心价值是把“让 AI 写代码”拆成更稳定、更可审查的流程：

```
任务分类
↓
方案文件
↓
方案审查
↓
代码实现
↓
实现报告
```

它解决的问题不是“让 AI 更快输出代码”，而是让 AI 在真实项目中更可靠地辅助编码，减少需求误判、上下文污染、实现偏离和后续难以审查的问题。