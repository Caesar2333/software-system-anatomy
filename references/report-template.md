# Report Template

Use this as the default output shape. The report must be scannable before it is complete. First give the essence, then the evidence.

## 0. 产品体验与能力拆解

Start here when the project is a product, app, firmware, framework, agent, or tool with an intended experience. Keep it short.

```text
它表面上是 [表面分类]，但真实体验是 [用户/系统最终得到的能力]。
所以它必须具备：[capability A]、[capability B]、[capability C]。
```

Use this table when it helps explain why modules exist:

| 产品/系统能力 | 为什么需要 | 对应模块 | 没有它会怎样 |
| --- | --- | --- | --- |
| 输入 |  |  |  |
| 运行时/控制 |  |  |  |
| 状态/记忆 |  |  |  |
| 输出/动作 |  |  |  |
| 通信 |  |  |  |
| 扩展 |  |  |  |
| 恢复 |  |  |  |

## 1. 30 秒总览

This section is mandatory. Keep it short enough to fit on one screen.

| Question | Answer |
| --- | --- |
| 这东西到底是什么 | 不是 [表面分类]，而是 [真实运行主线] |
| 它提供什么体验 | [actual product/user/system experience] |
| 系统怎么活着 | [entry/start] -> [runtime/loop/scheduler] |
| 数据怎么流 | [input] -> [dispatcher] -> [state/model] -> [actions] -> [output] |
| 谁控制谁 | [framework/runtime/orchestrator] controls [handlers/plugins/actions] |
| 状态在哪里 | [enum/store/context/db/device state] |
| 扩展点在哪里 | [plugin/hook/provider/adapter/tool/board] |
| 最大风险/边界 | [shared resource/failure mode/unfinished work] |
| 先读哪里 | [first file/module to read and why] |
| 特殊火花 | [the memorable project-specific insight] |

Then add three bullets:

```text
你只需要先记住：
1. [核心主线]
2. [核心关系]
3. [最特别的地方]
```

## 2. 架构预览图

For HTML reports, this section is mandatory and should appear before long prose. Use `architecture-canvas.md`.

The preview must show:

| Visual element | Required content |
| --- | --- |
| Nodes | external actors, adapters, runtime/control plane, state/model, action executors, resources, extension points, outputs |
| Edges | data flow, control flow, event/state flow, extension links, resource ownership, failure/recovery |
| Legend | high-contrast color/style meaning that exactly matches drawn lines |
| Main artery | the strongest visible path from input to output |
| Where to find it | repository folder/file mapping for important nodes |
| Evidence | file/module references for important nodes and edges |
| Interaction | draggable canvas viewport with zoom in/out, fit/reset, and expand controls |

Then add one interpretation line:

```text
这张架构图的核心意思是：[central runtime/control owner] 把 [inputs] 收束起来，围绕 [state/model] 调度 [actions/extensions/resources]。
```

## 3. 一句话主线

```text
这个项目的主线是：外部输入通过 [入口] 进入系统，由 [调度/运行时] 分发到 [核心处理器]，围绕 [核心状态/数据模型] 产生 [动作/副作用]，并通过 [扩展点/反馈机制] 维持运行。
```

Prefer a "not X, but Y" reframing when it clarifies the architecture:

```text
它表面上是 [X]，真正主线是 [Y]。
```

## 4. 核心动脉图

Prefer one compact diagram that combines data flow and control flow:

```text
[外部输入]
  -> [入口 / adapter]
  -> [运行时 / dispatcher]
  -> [状态 / 数据模型]
  -> [动作执行器]
  -> [输出 / 副作用]
  -> [反馈 / 恢复]
```

Name concrete files or modules below the diagram.

Do not rely on the diagram alone. Add a one-sentence interpretation:

```text
这张图的重点是：[runtime] 不是普通模块，而是把 [input/state/action/extensions] 收束到一起的控制面。
```

## 5. 母题映射表

This is the report's "aha" table. It maps the user's mental model to this project.

| 软件工程母题 | 这个项目里的对应物 | 为什么重要 |
| --- | --- | --- |
| 生命周期 |  |  |
| 主循环/调度器 |  |  |
| 数据入口 |  |  |
| 状态机/状态存储 |  |  |
| 事件/消息 |  |  |
| 动作执行器 |  |  |
| 资源边界 |  |  |
| 扩展点 |  |  |
| 失败恢复 |  |  |

## 6. 模块到文件夹/文件映射

This section is mandatory when source files are available. Use the same node names as the architecture preview.

| 架构节点 | 仓库位置 | 关键文件/类 | 在图里的角色 | 先读哪里 |
| --- | --- | --- | --- | --- |
|  |  |  |  |  |

If a node is conceptual rather than a single directory, say so:

```text
[node] 是一个跨文件概念，主要证据在 [file A], [file B], [file C]。
```

## 7. 关键文件夹剖析

This section is mandatory when analyzing a real repository. Explain what important folders actually contain and why they exist.

| 文件夹/路径 | 里面有什么 | 对应架构模块 | 作用 | 先读哪里 |
| --- | --- | --- | --- | --- |
|  |  |  |  |  |

Add notes for confusing folders:

```text
[folder] 不是一个单一模块，而是 [responsibility A] + [responsibility B] 的混合区；理解时应该先看 [file]。
```

## 8. 数据流转链路

Explain the main data path:

| Stage | What happens | Evidence |
| --- | --- | --- |
| 输入 |  |  |
| 标准化/解析 |  |  |
| 分发/决策 |  |  |
| 状态读写 |  |  |
| 动作/副作用 |  |  |
| 输出/反馈 |  |  |

If the project has several important flows, use a small flow table instead of long prose:

| Flow | Path | Why it matters |
| --- | --- | --- |
| 主数据流 |  |  |
| 控制流 |  |  |
| 事件流 |  |  |
| 失败流 |  |  |

## 9. 控制权、状态与事件

Answer:

- Who owns the main control flow?
- Is control direct or inverted?
- Which runtime/framework/scheduler calls user code?
- Where is the "brain" or dispatcher?

| State/Event | Meaning | Producer | Consumer/Handler | Effect |
| --- | --- | --- | --- | --- |
|  |  |  |  |  |

Mention if the project is mostly direct-call and does not need a heavy event model.

## 10. 模块为什么必须存在

First derive important modules from product/system capability:

| 能力压力 | 模块 | 为什么必须存在 | 设计代价 / 风险 |
| --- | --- | --- | --- |
|  |  |  |  |

Then group important modules by role. Do not list every file:

| Role | Modules/files | Why they exist |
| --- | --- | --- |
| Entry / adapter |  |  |
| Runtime / dispatcher |  |  |
| State holder |  |  |
| Action executor |  |  |
| Resource owner |  |  |
| Extension point |  |  |
| Recovery/feedback |  |  |

After the table, add one paragraph explaining the real hierarchy:

```text
最上层不是 [module A]，而是 [runtime/control plane]；[module B/C/D] 都是围绕它展开的适配器或执行器。
```

## 11. 扩展点、资源边界与失败恢复

Explain:

- Where new behavior can attach.
- What changes are isolated behind modules/adapters.
- Which resources are shared, scarce, or protected.
- What contracts extension authors or callers must satisfy.

Trace the failure path:

```text
failure/error/pressure
  -> detection
  -> handling decision
  -> retry/fallback/rollback/surface
  -> log/test/metric/user feedback
```

If failure handling is weak or absent, say so plainly.

## 12. 项目特殊火花

Use this section to make the report memorable:

- What matches the expected software-system model?
- What is unusually simple, elegant, or surprising?
- What project-specific idea would help the user understand similar projects later?
- Where would a generic architecture summary miss the point?

## 13. 建议阅读路线

Give a route, not a file dump:

1. Read [entrypoint] to see how the system wakes up.
2. Read [runtime/dispatcher] to understand who controls whom.
3. Read [core data/state] to understand the central model.
4. Read [main handlers/actions] to see work happen.
5. Read [extensions/adapters] to understand variation.
6. Read [failure/recovery/tests] to understand production behavior.

## 14. 修改入口与风险

Include this section when the user is learning the project to fork, customize, or continue development.

| 修改目标 | 应该先看/改哪里 | 主要风险 |
| --- | --- | --- |
|  |  |  |

## 15. 证据附录

Use this section only when the report is deep or when many claims need proof.

Group evidence by claim:

| Claim | Evidence | Confidence |
| --- | --- | --- |
|  | file/function/line | high/medium/low |

Do not push evidence into the first screen.
