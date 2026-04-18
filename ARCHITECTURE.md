# SymbolFlow — 生成性世界层 + 现实承诺层 架构文档

> "不是人在思考，不是 LLM 在联想。是某个东西借人和 LLM 的口在流，SymbolFlow 是那个东西流过的痕迹。"
>
> 上一层：SymbolFlow = 语言关联可视化工具（已实现）
> 本层：SymbolFlow/ CEE = 生成性世界层 + 现实承诺层（架构探索）

---

## 一、核心理念

### 1.1 三个项目的关系

| 项目 | 定位 | 当前状态 |
|------|------|----------|
| ShapeLab | 2D 画图 + 建模单文件工具 | 已完成，GitHub: github.com/chleya/ShapeLab |
| SymbolFlow（上层） | 语言关联可视化工具 | 已完成，GitHub: github.com/chleya/SymbolFlow |
| SymbolFlow（下层）+ CEE | 生成性世界层 + 现实承诺层 | 架构设计阶段，未实现 |

### 1.2 为什么不满足于"候选 / 审批 / 执行"

当前 CEE 的结构仍然很像**人类行政式信息处理**：

```
理解语言 → 生成候选 → 审批 → 执行 → 归档
```

这对于高风险执行有价值，但它不是最本体、最连续、最 AI-native 的信息处理方式。

更优雅的追求是：

* 连续场而不是离散表单
* 动态耦合而不是层层审批
* 吸引子收敛而不是候选列表
* 约束传播而不是手工判定
* 世界-行动-自身一体化演化

---

## 二、四大替代范式

### 范式 1：约束传播式

**本体起点：** 世界不是"对象 + 流程"，而是一组彼此制约的关系。

**核心单位：** 张力与相容性

**信息处理核心：** 维护约束网络的一致性，寻找可满足解

**优点：** 很本质，很适合结构一致性问题，很适合多目标协同
**缺点：** 对开放语义不够自然，创造性跳跃弱，容易过于静态

---

### 范式 2：吸引子 / 能量面式

**本体起点：** 世界、目标、自身、行动是一个高维动力系统中的态。

**核心单位：** 状态在结构场中的流动

**信息处理核心：** 系统如何从不稳定态流向稳定态（能量下降）

**优点：** 非常 AI-native，非常适合连续认知，能解释"灵感、收敛、顿悟"
**缺点：** 很难工程化，很难解释，很难控制，很难做安全边界

---

### 范式 3：世界模型式 ← 当前推荐

**本体起点：** 系统最底层围绕"对世界、自身、行动后果的内部模拟"。

**核心单位：** 可想象的状态演化

**信息处理核心：** 构建足够好的内部世界，在里面试探

**组织世界：** 物体 / 结构 / 因果关系；行动会如何改变世界；自己在世界中的位置；目标与环境之间的张力

**改造世界：** 先在内部模拟多个干预方案，比较后果后再执行

**改造自身：** 模拟"若我改变策略/表征/记忆，会怎样"

**优点：** 最接近真正智能，能处理不可言说的部分，适合世界组织/改造/自我改造三者统一
**缺点：** 极难做真，需要大量反馈和多模态交互，很容易只做成"假的世界模型"

---

### 范式 4：主动推断式

**本体起点：** 系统不问"我要做什么"，而问"我对世界和自身的预测，哪里错了"。

**核心单位：** 误差闭环

**信息处理核心：** 通过更新模型或改变世界来最小化预测失配

**优点：** 最统一，最连续，最接近"活系统"
**缺点：** 最抽象，最难落地，很容易写成哲学不容易做成系统

---

### 推荐结论

> **短中期最值得继续走的是：世界模型式。**

原因：它刚好站在工程可实现性、智能深度、未来可扩展性三者之间最平衡的位置。

---

## 三、世界模型式的双层结构

### 上层：SymbolFlow = 生成性世界层（Generative World Layer）

负责：

* 形成内部世界
* 维持内部流动结构
* 展开未来轨迹
* 吸收模糊意义
* 生成可试探的状态演化

**不是候选机，而是内部世界本身的生成器。**

---

### 下层：CEE = 现实承诺层（Reality Commitment Layer）

负责：

* 锚定现实片段
* 记录现实后果
* 把接触世界的动作变成承诺事件
* 让现实反过来修正内部世界

**不是审批机，而是内部世界与外部世界之间的承诺接口。**

---

### 两者分工

* **SymbolFlow** 保存**生成性表征**——还在内部流动、可以变化、可以合并、可以分叉
* **CEE** 保存**承诺性表征**——已经被外部观测、工具结果、行动后果、约束事实固定住的部分

---

## 四、六大数据结构（最小可落地代码）

### 4.1 共享协议层：`shared_protocol/world_schema.py`

```python
from __future__ import annotations
from dataclasses import dataclass
from typing import Literal, Tuple

Confidence = float

@dataclass(frozen=True)
class WorldEntity:
    entity_id: str
    kind: str  # object / agent / goal / tool / resource / region / concept
    summary: str
    confidence: Confidence = 1.0

@dataclass(frozen=True)
class WorldRelation:
    relation_id: str
    subject_id: str
    predicate: str  # causes / supports / blocks / part_of / similar_to / can_change
    object_id: str
    confidence: Confidence = 1.0

@dataclass(frozen=True)
class WorldHypothesis:
    hypothesis_id: str
    statement: str
    related_entity_ids: Tuple[str, ...] = ()
    related_relation_ids: Tuple[str, ...] = ()
    confidence: Confidence = 0.5
    status: Literal["active", "tentative", "stale", "rejected"] = "tentative"

@dataclass(frozen=True)
class RevisionDelta:
    delta_id: str
    target_kind: Literal[
        "entity_add", "entity_update", "entity_remove",
        "relation_add", "relation_update", "relation_remove",
        "hypothesis_add", "hypothesis_update", "hypothesis_remove",
        "goal_update", "tension_update", "anchor_add", "self_update",
    ]
    target_ref: str
    before_summary: str
    after_summary: str
    justification: str
```

---

### 4.2 SymbolFlow 侧：`symbolflow/world_state.py`

```python
from __future__ import annotations
from dataclasses import dataclass, replace
from typing import Optional, Tuple

from shared_protocol.world_schema import Confidence, WorldEntity, WorldHypothesis, WorldRelation

@dataclass(frozen=True)
class WorldState:
    state_id: str
    parent_state_id: Optional[str]

    entities: Tuple[WorldEntity, ...] = ()
    relations: Tuple[WorldRelation, ...] = ()
    hypotheses: Tuple[WorldHypothesis, ...] = ()

    dominant_goals: Tuple[str, ...] = ()
    active_tensions: Tuple[str, ...] = ()

    self_capability_summary: Tuple[str, ...] = ()
    self_limit_summary: Tuple[str, ...] = ()
    self_reliability_estimate: Confidence = 0.5

    anchored_fact_summaries: Tuple[str, ...] = ()
    provenance_refs: Tuple[str, ...] = ()

def add_anchor_facts(state, fact_summaries, provenance_ref, new_state_id):
    merged_facts = tuple(dict.fromkeys(state.anchored_fact_summaries + fact_summaries))
    merged_provenance = state.provenance_refs + (provenance_ref,)
    return replace(state,
        state_id=new_state_id,
        parent_state_id=state.state_id,
        anchored_fact_summaries=merged_facts,
        provenance_refs=merged_provenance)

def update_hypothesis_status(state, hypothesis_id, new_status, new_confidence, new_state_id, provenance_ref):
    updated = []
    for h in state.hypotheses:
        if h.hypothesis_id == hypothesis_id:
            updated.append(WorldHypothesis(
                hypothesis_id=h.hypothesis_id, statement=h.statement,
                related_entity_ids=h.related_entity_ids,
                related_relation_ids=h.related_relation_ids,
                confidence=new_confidence, status=new_status))
        else:
            updated.append(h)
    return replace(state,
        state_id=new_state_id, parent_state_id=state.state_id,
        hypotheses=tuple(updated),
        provenance_refs=state.provenance_refs + (provenance_ref,))
```

---

### 4.3 CEE 侧 A：`cee/commitment.py`

```python
from __future__ import annotations
from dataclasses import dataclass
from typing import Literal, Tuple

from symbolflow.world_state import WorldState

@dataclass(frozen=True)
class CommitmentEvent:
    event_id: str
    source_state_id: str

    commitment_kind: Literal["observe", "act", "tool_contact", "internal_commit"]

    intent_summary: str
    expected_world_change: Tuple[str, ...] = ()
    expected_self_change: Tuple[str, ...] = ()

    affected_entity_ids: Tuple[str, ...] = ()
    affected_relation_ids: Tuple[str, ...] = ()

    action_summary: str = ""
    external_result_summary: str = ""
    observation_summaries: Tuple[str, ...] = ()

    success: bool = True
    reversibility: Literal["reversible", "partially_reversible", "irreversible"] = "reversible"
    cost: float = 0.0
    risk_realized: float = 0.0

def make_observation_commitment(state, *, event_id, intent_summary, target_entity_ids=()):
    return CommitmentEvent(
        event_id=event_id,
        source_state_id=state.state_id,
        commitment_kind="observe",
        intent_summary=intent_summary,
        affected_entity_ids=target_entity_ids,
        action_summary="request observation from reality interface")
```

---

### 4.4 CEE 侧 B：`cee/revision.py`

```python
from __future__ import annotations
from dataclasses import dataclass
from typing import Literal, Tuple

from cee.commitment import CommitmentEvent
from shared_protocol.world_schema import RevisionDelta
from symbolflow.world_state import WorldState, add_anchor_facts, update_hypothesis_status

@dataclass(frozen=True)
class ModelRevisionEvent:
    revision_id: str
    prior_state_id: str
    caused_by_event_id: str

    revision_kind: Literal["confirmation", "correction", "expansion", "compression", "recalibration"]

    deltas: Tuple[RevisionDelta, ...] = ()

    discarded_hypothesis_ids: Tuple[str, ...] = ()
    strengthened_hypothesis_ids: Tuple[str, ...] = ()
    new_anchor_fact_summaries: Tuple[str, ...] = ()

    resulting_state_id: str = ""
    revision_summary: str = ""

def revise_from_commitment(state, event, *, revision_id, resulting_state_id,
                           strengthened_hypothesis_ids=(), discarded_hypothesis_ids=(),
                           new_anchor_fact_summaries=(), revision_summary=""):
    deltas = []
    for fact in new_anchor_fact_summaries:
        deltas.append(RevisionDelta(
            delta_id=f"delta-anchor-{len(deltas)+1}",
            target_kind="anchor_add", target_ref=fact,
            before_summary="fact not anchored", after_summary=fact,
            justification=f"anchored by event {event.event_id}"))
    for hid in strengthened_hypothesis_ids:
        deltas.append(RevisionDelta(
            delta_id=f"delta-hyp-strengthen-{hid}",
            target_kind="hypothesis_update", target_ref=hid,
            before_summary="hypothesis tentative/uncertain",
            after_summary="hypothesis strengthened",
            justification=f"supported by event {event.event_id}"))
    for hid in discarded_hypothesis_ids:
        deltas.append(RevisionDelta(
            delta_id=f"delta-hyp-discard-{hid}",
            target_kind="hypothesis_update", target_ref=hid,
            before_summary="hypothesis active/tentative",
            after_summary="hypothesis rejected",
            justification=f"contradicted by event {event.event_id}"))
    return ModelRevisionEvent(
        revision_id=revision_id, prior_state_id=state.state_id,
        caused_by_event_id=event.event_id,
        revision_kind="confirmation" if event.success else "correction",
        deltas=tuple(deltas),
        discarded_hypothesis_ids=discarded_hypothesis_ids,
        strengthened_hypothesis_ids=strengthened_hypothesis_ids,
        new_anchor_fact_summaries=new_anchor_fact_summaries,
        resulting_state_id=resulting_state_id,
        revision_summary=revision_summary)

def apply_revision(state, revision):
    new_state = state
    if revision.new_anchor_fact_summaries:
        new_state = add_anchor_facts(new_state,
            fact_summaries=revision.new_anchor_fact_summaries,
            provenance_ref=revision.caused_by_event_id,
            new_state_id=revision.resulting_state_id or f"{state.state_id}-rev")
    if new_state.state_id == state.state_id:
        from dataclasses import replace
        new_state = replace(new_state,
            state_id=revision.resulting_state_id or f"{state.state_id}-rev",
            parent_state_id=state.state_id,
            provenance_refs=new_state.provenance_refs + (revision.caused_by_event_id,))
    for hid in revision.strengthened_hypothesis_ids:
        new_state = update_hypothesis_status(new_state, hid, "active", 0.9,
            new_state.state_id, revision.caused_by_event_id)
    for hid in revision.discarded_hypothesis_ids:
        new_state = update_hypothesis_status(new_state, hid, "rejected", 0.0,
            new_state.state_id, revision.caused_by_event_id)
    return new_state
```

---

## 五、最小运行示例

```
WorldState-0 (内部有假设: 门是锁着的)
    ↓ make_observation_commitment
CommitmentEvent (观察门)
    ↓ 现实返回: 门可以被推开，没有锁
revise_from_commitment
ModelRevisionEvent (修正事件: 推翻原假设，新增锚定事实)
    ↓ apply_revision
WorldState-1 (内部更新: 门可以推开，无锁)
```

---

## 六、三组对照实验设计

### 实验 1：现实反馈是否真的能"修正模型"

**核心假设：** `ModelRevisionEvent` 不是高级日志，而是会改变后续行为的机制。

**任务域：** 规则核对任务——系统先形成"某条规则适用/不适用"的内部判断，再去查外部规范文本确认。

**三个组：**

* A 组：普通 tool-calling baseline（OpenAI function calling + Structured Outputs）
* B 组：LangGraph baseline（state + checkpoints + thread + replay，但不建 `ModelRevisionEvent`）
* C 组：新结构（`WorldState` + `CommitmentEvent` + `ModelRevisionEvent`）

**Stop / Go 判据：**

* Go：C 组在相似任务第二轮里，重复错误率比 A/B 低至少 20%，且调试时能明确指出"哪条内部假设被现实推翻"
* Stop：C 组的主要收益只是"日志更好看"，而不是"更少重犯错误"

---

### 实验 2：`WorldState` 是否真的优于"普通状态 + 分层记忆"

**核心假设：** `WorldState` 不是"状态容器 + 记忆容器"的高级叫法。

**任务域：** 多轮规则审查——系统必须在多轮里维持"当前世界是怎样的、哪些是假设、哪些已确认"。

**三个组：**

* A 组：Letta baseline（core memory blocks + archival memory 分层）
* B 组：LangGraph baseline（graph state + checkpoints + replay）
* C 组：`WorldState`（entities + relations + hypotheses + anchored facts + tensions）

**Stop / Go 判据：**

* Go：C 组在多轮任务里，事实/假设混淆率相比 A/B 显著下降，人工审查时更容易区分"已知"和"不确定"
* Stop：混淆率和漂移率没有明显改善，或维护成本明显高于 A/B

---

### 实验 3：这套新结构是否比"现成堆叠方案"更值得

**核心假设：** 新结构比 LangGraph + Letta memory + OpenAI tool calling 的堆叠方案有明确新能力。

**任务域：** 小但完整的"高约束多步任务"基准，天然包含：内部理解会变、需要现实接触、接触后下一步要被影响。

**两个组：**

* A 组：LangGraph + Letta core/armorial memory + OpenAI function calling / strict structured outputs
* B 组：`WorldState` + `CommitmentEvent` + `ModelRevisionEvent` + 现实接触接口 + 修正后回写

**Stop / Go 判据：**

* Go：B 组在以下任意两项上稳定优于 A 组：更低重复错误率、更快错误归因、更低事实/假设混淆、更清楚现实修正可解释性，且工程复杂度不能高出太多
* Stop：B 组的主要提升只体现在"理论上更清楚"，而不是任务表现和调试收益

---

## 七、统一实验规则

1. **不要用"我们自己最擅长的例子"当全部数据**——至少准备一组系统没见过但结构相似的任务
2. **不要只看一次成功**——必须看第二轮/第三轮任务表现
3. **不要只记录成功率**——重点看：减少重复错误、提高归因速度、降低事实/假设混淆
4. **必须记录工程成本**——如果收益只有一点点，但要多维护一大套本体对象，战略上不值

---

## 八、模块归属

| 模块 | 归属 | 职责 |
|------|------|------|
| `world_schema.py` | shared_protocol | 共享世界语法，两边都依赖 |
| `world_state.py` | SymbolFlow | 内部世界状态容器 |
| `commitment.py` | CEE | 现实承诺事件 |
| `revision.py` | CEE | 模型修正事件 |
| `simulation.py` | SymbolFlow | 内部模拟引擎（待实现） |
| `reality_interface.py` | CEE | 现实接触接口（待实现） |

---

## 九、最该砍掉什么

**第一刀：砍掉"完整世界层"的野心**

收缩到最小三元：
* 当前内部假设
* 当前待接触现实的意图
* 当前现实反馈后的修正

**第二刀：砍掉 `RevisionDelta` 的细粒度 ambitions**

先证明三件事：
1. 现实是否真的改变了内部判断
2. 这种改变是否能被稳定记录
3. 这种改变是否能减少重复错误

**第三刀：砍掉所有 trajectory 类对象**

**第四刀：砍掉所有和"自我"有关的叙事性结构**

只保留：`capability_summary` / `limit_summary` / `reliability_estimate`

---

## 十、什么时候说明它真的有效

1. 新框架下，系统是否更能明确区分：假设、锚定事实、已发生承诺的现实接触
2. 现实失败回来后，系统是否能明确写出：原模型错在哪、修正了什么、哪些假设被丢弃
3. 面对多步复杂任务时，系统是否更少出现"内部理解漂移"
4. 是否能把某一类错误定位到：内部世界建模错误、现实承诺错误、还是模型修正错误

---

## 十一、什么时候应该停掉这条主线

出现以下任一情况，应停掉"生成性世界层 + 现实承诺层"：

* 重复错误率没有显著下降
* 事实/假设混淆率没有显著下降
* 调试和错误归因没有明显变快
* 相比 LangGraph/Letta/普通 tool-calling 的堆叠方案，没有形成明确新能力
* 工程复杂度明显上升，但收益不成比例

**只有当它至少在"少重犯错 + 更快归因 + 更清楚区分内部假设与现实事实"这三件事里，明显赢下两件，才值得继续。**

---

*最后更新：2026-04-18*
