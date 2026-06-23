# OIRF: Open Industry Research Format

> 中文 | [English](#oirf-open-industry-research-format-english)

## OIRF：开放行业研究格式

OIRF（Open Industry Research Format，开放行业研究格式）是一个面向 AI 原生行业研究的开放格式标准草案，旨在让行业研究中的报告正文、事实、观点、理论、证据、信源、假设、责任人、人机协作过程和版本变化变得可追溯、可审计、可协作和可复用。

OIRF 由头豹研究院发起，并由弗若斯特沙利文中国参与共建。目前项目处于 v0.1 概念验证与社区讨论阶段，后续计划探索以开源规范、行业共识文件或团体标准等方式持续推进。

OIRF 并不试图把行业报告简单改造成数据库，也不要求所有研究材料默认公开。它关注的是：如何用开放、可读、可扩展的方式承载行业研究的完整知识结构，让研究成果既能被人类阅读，也能被机器理解、校验、复用和持续维护。

## 为什么需要 OIRF

传统行业研究通常依赖 Word、PPT、Excel、聊天记录和本地文件夹进行协作。随着项目积累，报告正文、数据来源、研究判断、专家意见、模型假设、AI 辅助过程和版本变更会逐渐分散，难以复盘、审计、复用和自动化。

OIRF 希望解决五个问题：

- 关键结论能否追溯到来源、证据和适用边界；
- 事实、观点和理论框架能否分离，避免口径混淆；
- 人类研究者、审核者、组织和 AI 系统的参与过程能否显式记录；
- 结构化推理内容能否进行语义化版本比较，而不只依赖文本 diff；
- 研究资产能否被大模型、知识图谱、数据库和企业知识库复用。

行业研究中真正有价值的，往往不只是某一时刻的判断，而是判断为什么从 A 变成 B，中间新增了什么证据、推翻了什么假设、调整了什么口径。OIRF 通过版本层与语义化 diff，把这些原本隐藏在研究员经验和沟通记录中的变化过程，转化为可审计、可计算、可复用的知识历史。

## 三层架构

OIRF 将行业研究拆分为三层：

1. **呈现层 Presentation Layer**  
   使用 Markdown 保持报告的自然阅读体验，承载摘要、市场定义、规模测算、竞争格局、驱动因素、风险提示等传统报告内容。

2. **推理层 Reasoning Layer**  
   使用 JSON 结构化记录事实、观点、理论、证据、来源、参与者和研究任务，使关键判断具备机器可读、可审计、可复用的底层语义。

3. **版本层 Version Layer**  
   使用 Git 管理 Markdown 文本历史，同时使用语义化 tree diff 记录推理层 JSON 的变化。它关注的不是文本哪一行被修改，而是哪个事实被更新、哪个观点的置信度被下调、哪个理论框架被替换、哪个证据改变了结论。

简单来说：

```text
Markdown 负责“怎么读”
JSON 负责“为什么这么判断”
Semantic Diff 负责“判断如何变化”
```

版本层让 OIRF 从“知识快照格式”升级为“知识演化格式”。

## 核心实体

OIRF v0.1 的推理层以三类核心实体为基础：

- **Fact 事实**：可核验、可引用、可被证伪的观察、数据或事件；
- **Opinion 观点**：解释、判断、预测、建议或假设；
- **Theory 理论**：用于解释事实和生成观点的框架、模型或机制。

同时保留四类支撑实体：

- **Source 信源**：原始材料、访谈、数据库、文件或 AI 输出；
- **Evidence 证据**：从信源中抽取并用于支持或反驳结论的证据单元；
- **Actor 参与者**：研究员、审核者、组织或 AI 系统；
- **Task 任务**：研究任务、人机协作记录和审核流程。

推荐关系：

```text
source -> evidence -> fact -> opinion -> report
                    theory -> opinion

actor -> task -> review -> semantic_diff
```

## Markdown 标注示例

Markdown 作为呈现层，保持自然阅读体验，只在关键位置挂接推理层 ID。

```md
2023 年中国变频空气能市场规模约为 114.6 亿元。  
[#fact:F-2023-vf-market-size] [#evidence:E-slide8-chart]

我们判断，变频空气能市场增速高于整体空气能市场，主要来自变频渗透率提升。  
[#opinion:O-penetration-growth] [#theory:T-penetration-upgrade]
```

Markdown 不需要承载全部结构。它只负责让读者知道哪些句子可以追溯到推理层。

## Reasoning JSON 示例

```json
{
  "id": "fact:F-2023-vf-market-size",
  "type": "fact",
  "statement": "2023年中国变频空气能市场规模约为114.6亿元。",
  "status": "reviewed",
  "measurement": {
    "metric": "market_size",
    "value": 114.6,
    "unit": "亿元",
    "period": "2023",
    "geography": "中国大陆",
    "segment": "变频空气能"
  },
  "evidenceIds": ["evidence:E-slide8-chart"],
  "sourceIds": ["source:S-air-source-ppt"],
  "ownerActorId": "actor:A-data-owner",
  "confidence": {
    "level": "medium",
    "rationale": "来自报告可见图表，但原始底稿不可见。"
  }
}
```

## Semantic Diff 示例

```json
{
  "id": "diff:D-0001",
  "type": "semantic_diff",
  "baseVersion": "0.1.0",
  "targetVersion": "0.1.1",
  "changedByActorId": "actor:A-report-owner",
  "changes": [
    {
      "changeType": "field_changed",
      "entityId": "opinion:O-penetration-growth",
      "path": "/confidence/level",
      "from": "medium",
      "to": "low",
      "reason": "新增反证显示2024年实际销量可能低于预测。"
    }
  ]
}
```

这类 diff 记录的不是“第几行文本变了”，而是“哪个研究判断为什么变了”。

## 仓库内容

当前仓库计划包含：

- `SPEC.md`：OIRF v0.1 标准说明与规范草案；
- `TAGGING.md`：Markdown 标注规则；
- `schemas/reasoning.schema.json`：Reasoning Layer JSON Schema；
- `schemas/semantic-diff.schema.json`：Semantic Diff JSON Schema；
- `examples/`：最小化示例报告包；
- `report/`：Markdown 呈现层示例；
- `reasoning/`：事实、观点、理论、信源、证据、参与者和任务示例；
- `versions/`：语义化版本变化示例。

推荐目录结构：

```text
open-report/
  oirf.json
  report/
    index.md
    01-summary.md
    02-market-size.md
    03-competition.md
  reasoning/
    facts.json
    opinions.json
    theories.json
    sources.json
    evidence.json
    actors.json
    tasks.json
  versions/
    reasoning-diff-0001.json
```

## OIRF 不是什么

OIRF 不是行业报告模板，不规定报告必须如何写作。

OIRF 不是数据库产品，不替代现有知识库、BI、文档系统或报告生产工具。

OIRF 不要求公开所有原始材料、访谈记录、客户文件或 AI 对话。

OIRF 不保证研究结论自动正确。它只是让结论更容易被追溯、检查、复用和更新。

## 当前状态

OIRF 目前处于：

```text
Status: Draft / Proof of Concept
Version: v0.1
```

当前阶段重点讨论：

- 三层架构是否合理；
- `fact / opinion / theory` 三类核心实体是否足够稳定；
- Markdown 标注方式是否适合真实报告写作；
- Reasoning JSON Schema 是否能覆盖行业研究核心场景；
- Semantic Diff 是否能表达结论、证据、假设和置信度变化；
- 如何与 JSON-LD、RDF、知识图谱和企业知识库对接。

## 路线图

- v0.1：完成格式草案、Schema、标注规范和最小示例；
- v0.2：增加真实行业报告样例和更多行业场景；
- v0.3：补充 JSON-LD / RDF 映射和知识图谱导出；
- v0.4：开发基础校验工具和 Markdown tag 检查器；
- v1.0：形成稳定规范，推动社区评审和标准化讨论。

## 参与贡献

欢迎以下方向的贡献：

- Schema 设计建议；
- Markdown 标注语法改进；
- 语义化 diff 规则设计；
- 行业报告示例；
- JSON-LD / RDF 映射；
- AI 协作记录规范；
- 证据审计和权限管理实践；
- 多语言文档翻译。

你可以通过 Issue、Pull Request 或讨论区提出建议。

## 许可建议

建议采用：

- 文档：Creative Commons Attribution 4.0 International（CC BY 4.0）
- Schema / 示例代码：Apache License 2.0 或 MIT License

如果示例报告基于受限材料、客户材料或商业报告重构，应单独标注披露等级和使用权限。
