| name        | coding-skill                                                 |
| ----------- | ------------------------------------------------------------ |
| description | 当用户希望新建代码项目、修改已有项目、添加功能、修改功能、修复 Bug、重构优化，或者根据已有方案文件写代码时，使用本 skill。 |
| version     | 1.0.0                                                        |
| author      | xibeidi_1_深情                                               |

## 什么时候使用本 skill

当用户请求涉及以下任意情况时，使用本 skill：

- 新建一个代码项目
- 从 0 到 1 创建项目
- 给已有项目添加功能
- 修改已有项目中的功能行为
- 修复已有项目中的 Bug
- 重构或优化已有项目代码
- 根据已有方案文件写代码
- 根据已有方案文件实现功能
- 根据已有方案文件修改项目
- 根据已有方案文件修复 Bug
- 根据已有方案文件进行重构优化

典型用户请求：

```
帮我做一个新项目
从零创建一个任务管理系统
给这个项目加一个导出功能
把登录逻辑改一下
这个接口有 bug，帮我修
这个项目跑不起来
这个函数太长，帮我重构
根据 docs/ai-plans/add-feature/export_excel.md 写代码
开始实现刚才的方案
按方案文件落地到代码里
```

------

## 什么时候不要使用本 skill

以下情况不使用本 skill：

- 用户只是询问编程概念
- 用户只是询问某个 API 的用法
- 用户只是要求解释一段代码
- 用户只是要求写一个孤立代码片段
- 用户只是要求算法题解
- 用户只是要求格式化、翻译、总结非项目代码内容
- 用户请求和代码项目改动无关

例如：

```
Python 的 list 和 tuple 有什么区别？
解释一下这段代码
帮我写一个冒泡排序
这个正则是什么意思？
```

这些请求可以直接回答，不需要进入本 skill 的 reference 流程。

------

## 总体原则

本 skill 的核心原则是：

```
先判断任务类型，再进入对应 reference。
```

不要在 `SKILL.md` 中直接生成方案、直接写代码或直接修改文件。

`SKILL.md` 只负责：

1. 判断用户是否在请求代码项目相关工作
2. 判断用户请求属于哪一种类型
3. 读取并执行对应 reference
4. 保持方案设计和代码实现分离

------

## Reference 列表

本 skill 使用以下 reference：

```
references/common_requirement_clarification.md

references/new_project_plan.md

references/old_project_add_feature.md
references/old_project_modify_feature.md
references/old_project_fix_bug.md
references/old_project_refactor_optimize.md

references/implement_from_plan.md
```

对应 assets：

```
assets/new_project_plan_template.md

assets/add_feature_plan_template.md
assets/modify_feature_plan_template.md

assets/fix_runtime_bug_template.md
assets/fix_behavior_bug_template.md

assets/refactor_optimize_plan_template.md

assets/implementation_report_template.md
```

------

## 分流规则

### 1. 新项目：从 0 到 1 创建项目

当用户希望创建一个新项目，且当前没有可继续开发的完整项目代码时，转入：

```
references/new_project_plan.md
```

典型判断：

- 用户说要新建项目
- 用户说从零开始
- 用户只有项目想法，没有项目结构
- 用户希望先确定项目目标、MVP、技术栈、模块划分、目录结构和开发顺序

示例：

```
我要做一个课程自动化系统
帮我从零创建一个库存管理项目
我想做一个数据清洗工具，先给我方案
```

------

### 2. 老项目：添加功能

当用户希望在已有项目中新增一个原本不存在的功能能力时，转入：

```
references/old_project_add_feature.md
```

典型判断：

- 项目已经存在
- 用户要新增能力
- 原项目没有这个功能
- 目标是把一个新功能接入旧项目

示例：

```
给现有项目加一个 Excel 导出功能
加一个任务暂停按钮
新增一个消息通知功能
后台加一个筛选条件
```

------

### 3. 老项目：修改功能

当用户希望改变已有功能的行为、流程、接口、数据结构、状态流转或执行方式时，转入：

```
references/old_project_modify_feature.md
```

典型判断：

- 项目已经存在
- 功能也已经存在
- 用户希望改变旧功能的行为
- 需要分析旧行为、新行为、差异、兼容和影响范围

示例：

```
原来只能单课程，现在改成多课程
登录逻辑要改一下
失败后不要直接结束，改成自动重试
接口返回格式从 A 改成 B
```

------

### 4. 老项目：修复 Bug

当用户希望修复已有项目中的错误、异常或不符合预期的行为时，转入：

```
references/old_project_fix_bug.md
```

典型判断：

- 程序跑不起来
- 命令执行失败
- 构建失败
- 测试失败
- 接口报错
- 页面报错
- 程序能运行，但结果不符合预期
- 用户认为现有功能表现错误

示例：

```
这个接口返回 500
项目启动失败
任务状态一直卡住
点击保存后数据库没有记录
页面显示的数据不对
```

注意：

修复 Bug reference 内部会继续判断：

```
运行类 Bug
行为类 Bug
```

并选择对应 assets。

------

### 5. 老项目：重构优化

当用户希望在功能行为不变的前提下优化已有项目代码时，转入：

```
references/old_project_refactor_optimize.md
```

典型判断：

- 功能当前是正确的
- 用户不希望改变业务行为
- 用户希望改善代码结构、可读性、可维护性、性能或测试结构

示例：

```
这个函数太长，帮我重构
这段代码重复太多，抽公共逻辑
这个接口能跑，但太慢了，优化一下
把 view 里的业务逻辑抽到 service
```

核心判断：

```
功能行为不变，只改善实现。
```

------

### 6. 根据方案文件实现代码

当用户明确要求根据已有方案文件写代码、开始实现、落地方案或修改项目时，转入：

```
references/implement_from_plan.md
```

典型判断：

- 用户说“开始实现”
- 用户说“根据方案写代码”
- 用户给出了 `docs/ai-plans/...` 路径
- 用户要求把之前生成的方案落地到代码
- 用户要求根据方案文件修复 Bug、添加功能、修改功能或重构优化

示例：

```
根据 docs/ai-plans/add-feature/export_excel.md 写代码
开始实现刚才的方案
按方案文件落地
根据这个 Bug 修复方案改代码
```

重要规则：

如果用户要求写代码，但没有明确方案文件路径，必须先进入 `references/implement_from_plan.md`，由该 reference 确认本次要根据哪个方案文件实现。

不要在未确认方案文件的情况下直接写代码。

------

## 优先级规则

当用户请求同时可能匹配多个类型时，按以下优先级判断。

### 第一优先级：写代码 / 实现方案

如果用户明确说：

```
写代码
开始实现
落地方案
按方案改代码
根据方案文件实现
```

优先转入：

```
references/implement_from_plan.md
```

因为此时用户目标是实现已有方案，而不是重新设计方案。

------

### 第二优先级：新项目

如果用户明确表示从零开始创建项目，转入：

```
references/new_project_plan.md
```

------

### 第三优先级：老项目中的具体变更

如果是已有项目，再判断具体类型：

```
新增能力 → references/old_project_add_feature.md
改变已有行为 → references/old_project_modify_feature.md
修复错误行为 → references/old_project_fix_bug.md
行为不变的优化 → references/old_project_refactor_optimize.md
```

------

## 模糊请求的处理

如果用户请求不够明确，不要直接生成方案或写代码。

应先进入对应 reference，由该 reference 执行：

```
references/common_requirement_clarification.md
```

来确认最小必要信息。

例如用户说：

```
帮我改一下项目
帮我优化一下
这个有问题
帮我做一下
```

如果无法判断属于添加、修改、修 Bug、重构还是实现，应先询问最小必要信息。

推荐回复：

```
我需要先确认这次任务类型：

1. 是从零创建新项目？
2. 是给已有项目添加新功能？
3. 是修改已有功能行为？
4. 是修复 Bug？
5. 是在功能不变的前提下重构优化？
6. 还是根据已有方案文件写代码？

请说明属于哪一种，或提供对应方案文件路径。
```

------

## 方案文件路径约定

方案类 reference 会生成方案文件到：

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

------

## 实现报告路径约定

实现类 reference 会生成实现报告到：

```
docs/ai-implementation/
```

路径规则：

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

```
docs/ai-plans/fix-bug/runtime/startup_error.md
↓
docs/ai-implementation/fix-bug/runtime/startup_error.md
```

------

## 执行要求

当决定进入某个 reference 后，必须读取并遵守该 reference。

不要只根据 `SKILL.md` 自行发挥。

执行顺序：

```
判断用户请求
↓
选择 reference
↓
读取 reference
↓
按 reference 执行
↓
使用对应 assets 创建或更新文件
```

------

## 禁止行为

使用本 skill 时禁止：

- 在没有读取对应 reference 的情况下直接输出方案
- 在没有确认方案文件的情况下直接写代码
- 把方案设计和代码实现混在一个步骤里
- 把方案内容长期堆在聊天上下文里
- 在聊天中反复输出整份方案文件
- 在实现阶段重新设计方案
- 在方案阶段直接写代码
- 跳过需求明确性检查
- 跳过项目上下文阅读
- 随意选择 reference
- 将添加功能、修改功能、修 Bug、重构优化混为一类

------

## 总结

本 skill 的核心分工是：

```
SKILL.md
负责判断任务类型和选择 reference

方案类 reference
负责创建 docs/ai-plans/.../*.md

实现类 reference
负责根据 docs/ai-plans/.../*.md 写代码，并创建 docs/ai-implementation/.../*.md
```

核心原则：

```
先分流，再执行 reference。
先方案，再实现。
实现必须基于明确的方案文件。
```