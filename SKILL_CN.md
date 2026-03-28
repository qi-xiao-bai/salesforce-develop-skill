---
name: salesforce-develop
description: Salesforce 项目开发规范与协作工作流。Use when Codex is asked to develop, modify, debug, refactor, review, or maintain Salesforce code or metadata in an existing project, especially Apex classes, triggers, controllers, schedulers, batch jobs, object fields, approval flows, OA integration, or other business process code. This skill enforces a mandatory pre-read of the matching project-root '*-开发文档.md' file before editing, preserves user-owned progress documentation, applies Chinese comments and debug conventions, supports controlled source backups, and forbids running any sfdx commands.
---

# Salesforce Develop

按下面的流程执行 Salesforce 开发任务。默认目标不是“尽快改完”，而是先理解上下文、再稳妥改动，并让后续排查与协作都更轻松。

英文版主文件请看 [SKILL.md](SKILL.md)。

## 参考资源

- 需要维护或新建开发文档时，先读取 [references/开发文档模板.md](references/开发文档模板.md)。
- 如果需要英文版模板，读取 [references/development-doc-template.md](references/development-doc-template.md)。
- 当用户要求“同步开发文档”“补写开发文档”“维护开发文档”时，优先按该模板组织内容，但仍然以当前项目实际代码和用户确认过的逻辑为准。
- 当用户明确要求备份文件时，优先使用 [scripts/backup_source_file.py](scripts/backup_source_file.py) 执行备份与轮转，不要手工重复实现一遍。

## 核心规则

1. 在任何编辑开始前，先定位项目根目录下与当前任务最相关的 `*-开发文档.md`，完整阅读后再动代码。
2. 如果找不到对应的 `*-开发文档.md`，停止编辑，明确告诉用户缺少开发文档，等待用户补充或明确授权放宽约束。
3. `*-开发文档.md` 不是自动维护文件。只有用户明确要求“维护文档”“更新开发文档”“同步开发文档”等意思时，才允许修改它。
4. 严格禁止执行任何 `sfdx` 命令，尤其是拉取、部署、推送、同步、鉴权、测试执行等命令。不要尝试用别名或等价命令绕过这条规则。
5. 开发中统一使用中文注释。
6. 每个新增或修改过的类、接口、枚举、触发器、方法，都写清晰的中文注释。
7. 对关键业务逻辑、边界判断、状态流转、批量处理、递归保护、汇总计算、异步链路等重要部分补充中文注释；不要给显而易见的赋值或 getter/setter 写低价值注释。
8. 在关键逻辑处添加 `System.debug` 便于观察，但不要重复堆叠。进入某个逻辑块时只在“进入后”打一次，不要进入前后各打一条同义 debug。

## 标准工作流

### 1. 先识别项目根目录

优先从用户给出的路径、当前工作目录、仓库标志文件（如 `.git/`、`sfdx-project.json`、`package.json`、`README.md`、`force-app/`）判断项目根目录。

### 2. 在项目根目录查找开发文档

按下面顺序查找：

1. 优先找和当前任务语义最接近的 `*-开发文档.md`
2. 如果存在多个候选，选择名称最匹配当前任务的那个
3. 如果仍不确定，先把候选列表告诉用户，再等待确认
4. 如果一个都没有，禁止直接编辑代码

任务与文档名称的匹配示例：

- 审批流开发 -> `审批流-开发文档.md`
- 合同会审开发 -> `合同会审-开发文档.md`
- OA 推送开发 -> `OA推送-开发文档.md`

### 3. 读完开发文档后再制定修改方案

至少提取这些信息：

- 当前开发进度
- 已有实现范围
- 已确认的业务规则
- 用户希望的完成形态
- 已知风险、遗留问题、待办项

如果文档内容与现有代码明显不一致，优先以“代码现状 + 用户最新口头要求”为准，并把冲突点明确告诉用户；不要擅自改写开发文档。

### 4. 再开始代码修改

修改时优先做到：

- 先读现有实现，再决定是增量修改还是局部重构
- 保持原有项目风格和命名习惯
- 不为了“看起来更优雅”做无关重构
- 不删除与当前任务无关的逻辑、注释、debug、测试或配置

## 开发文档维护规则

只有在用户明确要求维护开发文档时，才允许更新 `*-开发文档.md`。

维护时遵循下面规则：

1. 以“当前逻辑已经被用户认可”作为前提。
2. 如果实现了文档里原本没有记录的功能，直接补写进去，不要只写一句“已完成”。
3. 先从大结构写起，先说明入口、分层、职责边界和主链路，再展开到流程和方法。
4. 具体写清楚涉及的核心类、核心触发器、核心方法、入口、调用关系、关键字段、状态流转、边界处理。
5. 如果类很多，不需要把每一个类、每一个方法都逐条解释，只保留和当前开发或维护最相关的核心类、核心方法即可。
6. 如果是大功能，必须写出完整流程链，不要只写结果。
7. 流程部分不能只写中文业务介绍，必须同时写出方法调用链路。
8. 方法调用链路不要只写方法名列表；每一行都补中文解释，写明入参、出参和作用。
9. 像“Salesforce 审批业务场景映射”“审批动作阶段真实执行流程”这类特定场景章节不是模板强制项，只有当前任务确实需要时再加。

大结构示例：

```text
审批提交功能可先按下面几层理解：
- Controller：接收页面或接口请求，负责参数包装和结果返回
- Gateway / Facade：统一提交入口，负责分发到具体服务
- SubmitService：执行业务主链路，负责校验、流程匹配、实例创建
- Trigger / Handler：在记录落库后补充衍生逻辑或状态同步
- Notification / Async Service：负责待办、通知或后续异步处理
```

大功能流程链示例：

```text
审批提交
1. 前端或调用方发起提交
2. Controller 接收请求并完成基础参数包装
3. Gateway 或 Service 执行提交前校验与流程匹配
4. Service 创建审批主记录、步骤实例及相关运行时数据
5. Trigger 或 Handler 在记录落库后补充状态、节点或派生逻辑
6. Notification / Async Service 写入待办、通知或后续异步任务
7. 返回提交结果并更新页面状态
```

方法调用链路示例：

```text
整体链路说明：
页面提交请求先进入 Controller，随后由 Gateway 或主服务收口，再进入提交主链路。核心校验、流程匹配、审批实例创建完成后，再生成待办或通知，最后把提交结果返回前端。

页面按钮点击
-> `ApprovalSubmitController.submitApproval(submitRequest)`
   - 入参：`submitRequest`
   - 出参：`SubmitResult`
   - 作用：接收前端提交请求，完成参数包装、基础校验和结果返回
-> `CustomApprovalGateway.process(processRequest, allOrNone)`
   - 入参：`processRequest`、`allOrNone`
   - 出参：`ProcessResult`
   - 作用：统一提交入口，负责把请求转发到审批提交主服务
-> `CustomApprovalSubmitService.submit(processRequests, allOrNone)`
   - 入参：`processRequests`、`allOrNone`
   - 出参：`List<ProcessResult>`
   - 作用：执行业务主链路，控制整批提交、失败回滚和结果汇总
-> `CustomApprovalSubmitService.buildSubmitContext(processRequest)`
   - 入参：`processRequest`
   - 出参：`SubmitContext`
   - 作用：完成提交前校验、流程匹配、步骤匹配和运行时上下文组装
-> `CustomApprovalSubmitService.createInstancesForContexts(contexts)`
   - 入参：`contexts`
   - 出参：审批实例集合或回填后的上下文
   - 作用：创建审批主记录并建立本次提交流程实例
-> `CustomApprovalSubmitService.createInitialAssignmentsForContexts(contexts)`
   - 入参：`contexts`
   - 出参：待办分配结果或无返回
   - 作用：按步骤模式创建首步审批待办
-> `ApprovalEngineSupport.sendApprovalNotifications(assignments, recordId)`
   - 入参：`assignments`、`recordId`
   - 出参：无或通知执行结果
   - 作用：发送审批通知或补充待办提醒
-> 返回 `SubmitResult`
   - 作用：把最终提交结果返回给页面并驱动前端状态更新
```

开发文档建议至少覆盖这些信息：

- 功能目标
- 当前完成情况
- 大结构与职责边界
- 核心类与职责
- 关键方法与入参/出参
- 关键对象、字段、状态
- 完整流程链
- 方法调用链路
- 待完善项
- 已知风险或注意点

## 代码注释规范

### 类注释

每个新增或修改过的类、接口、枚举、触发器，都补中文注释，说明：

- 这个类或触发器做什么
- 主要处理哪个业务场景
- 有没有特殊限制或关键依赖

示例：

```java
/**
 * 审批提交流程服务类，负责处理提交前校验、审批记录写入及结果返回。
 * 适用于合同会审场景，依赖 Approval_Request__c 与 Approval_Step__c 对象。
 */
public with sharing class ApprovalSubmitService {
}
```

### 方法注释

每个新增或修改过的方法都写中文注释，至少说明：

- 方法用途
- 核心参数
- 返回值
- 关键副作用或注意事项

示例：

```java
/**
 * 根据合同记录发起审批提交。
 *
 * @param contractId 合同记录 Id
 * @param submitterId 提交人 Id
 * @return 提交结果，包含是否成功及提示信息
 */
public static SubmitResult submitApproval(Id contractId, Id submitterId) {
}
```

### 逻辑注释

对下面这些地方优先加注释：

- 状态判断
- 角色分支
- 批量处理
- 汇总计算
- 递归保护
- 异步调用
- 外部系统交互
- 容易误解的历史兼容逻辑

不要对明显语句写低价值注释，例如：

- 给变量赋值
- 循环列表
- 返回结果

## Debug 规范

在重要逻辑节点补充 `System.debug`，方便定位链路。重点放在：

- 方法真正进入关键分支之后
- 关键参数整理完成之后
- DML 前的核心数据状态
- DML 或调用后的关键结果
- 异常捕获处
- 汇总计算结果
- 外部接口请求与响应摘要

避免重复 debug：

- 不要在进入逻辑前写一条“准备进入”
- 紧接着进入逻辑后又写一条“已经进入”
- 二选一，只保留进入逻辑后的那条

推荐写法：

```java
System.debug(LoggingLevel.INFO, '进入审批提交主流程，contractId=' + contractId + ', submitterId=' + submitterId);
System.debug(LoggingLevel.INFO, '审批节点构建完成，stepCount=' + approvalSteps.size());
System.debug(LoggingLevel.ERROR, '审批提交异常，message=' + ex.getMessage());
```

避免用没有信息量的 debug，例如：

- `System.debug('111');`
- `System.debug('进入方法');`

## 备份规则

只有在用户明确提出“备份某个文件”时，才执行备份。

备份规则如下：

1. 备份目录固定为项目根目录下的 `备份` 文件夹
2. 如果 `备份` 文件夹不存在，则创建它
3. 备份文件名格式为：`原文件名-时间戳.原扩展名`
4. 时间戳建议使用 `yyyyMMdd-HHmmss`
5. 同一个源文件最多保留 3 份备份
6. 如果第 4 次备份同一个文件，先删除时间最久的那一份，再写入最新备份
7. 备份的是修改前原始代码，不是修改后的结果
8. 如果脚本可用，优先调用 `scripts/backup_source_file.py`

文件名示例：

```text
ApprovalSubmitService-20260328-221530.cls
ApprovalSubmitServiceTest-20260328-221715.cls
```

## Salesforce 开发建议

默认优先遵守这些习惯，除非用户明确要求例外：

- 优先按“触发器薄、处理器厚、服务层清晰”的结构思考
- 先考虑批量场景，再写单记录逻辑
- 涉及触发器时注意递归、重复 DML、查询次数与 governor limits
- 涉及审批、通知、回调、定时任务时，把完整链路想清楚再改局部代码
- 对影响状态流转的逻辑，优先补日志、注释和必要保护
- 对关键 SOQL 条件、Map 聚合和 Set 去重写明业务意图
- 如果发现现有代码风格混乱，先以“最小可控修改”为原则

## 禁止事项

绝对不要做下面这些事：

- 未读 `*-开发文档.md` 就开始编辑
- 未经用户明确要求就修改开发文档
- 执行任何 `sfdx` 命令
- 擅自部署、拉取、推送、认证、同步环境
- 擅自删除用户已有 debug、注释、旧逻辑或备份
- 为了“整洁”删除用户暂时保留的兼容代码，除非用户明确同意

## 交付时的默认说明

完成开发后，默认向用户说明：

- 本次读取了哪个 `*-开发文档.md`
- 实际修改了哪些类、触发器、方法或文件
- 哪些关键逻辑增加了中文注释
- 哪些关键节点增加了 debug
- 如果没有维护开发文档，要明确说明“按规则未自动更新开发文档”

如果用户明确要求同步开发文档，再补充说明文档中更新了哪些流程、类和方法。
