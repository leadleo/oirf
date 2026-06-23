# OIRF: Open Industry Research Format (English)

OIRF (Open Industry Research Format) is a draft open format standard for AI-native industry research. It is designed to make report narratives, facts, opinions, theories, evidence, sources, assumptions, responsible actors, human-AI collaboration and version history traceable, auditable, collaborative and reusable.

OIRF is initiated by LeadLeo Research Institute with participation from Frost & Sullivan China. The project is currently at the v0.1 proof-of-concept and community discussion stage. Future work may explore open specifications, industry consensus documents or formal group standards.

OIRF does not try to turn industry reports into simple databases. It also does not require every source material to be public by default. Its goal is to provide an open, readable and extensible way to represent the full knowledge structure behind industry research, so that research outputs can be read by humans and understood, checked, reused and maintained by machines.

## Why OIRF

Traditional industry research often relies on Word, PowerPoint, Excel, chat records and local folders. Over time, report text, data sources, expert inputs, model assumptions, AI assistance and version changes become scattered across files and conversations. This makes review, reuse, collaboration and automation difficult.

OIRF aims to answer five questions:

- Can every key conclusion be traced to its sources, evidence and applicability boundary?
- Can facts, opinions and theories be separated to reduce scope confusion?
- Can the work of human researchers, reviewers, organizations and AI systems be explicitly recorded?
- Can structured reasoning be compared semantically, instead of relying only on line-based text diffs?
- Can research assets be reused by large language models, knowledge graphs, databases and enterprise knowledge systems?

The most valuable part of industry research is often not only what an analyst believes at one moment, but why a judgment changed from A to B: what evidence was added, what assumption was rejected, what scope changed and who reviewed the change. OIRF turns these hidden research changes into auditable, computable and reusable knowledge history.

## Three-Layer Architecture

OIRF has three layers:

1. **Presentation Layer**  
   Markdown keeps the report readable. It carries executive summaries, market definitions, market sizing, competition analysis, drivers, risks and other traditional report sections.

2. **Reasoning Layer**  
   JSON records facts, opinions, theories, evidence, sources, actors and research tasks. It makes key judgments machine-readable, auditable and reusable.

3. **Version Layer**  
   Git tracks Markdown history, while semantic tree diff records changes in structured reasoning JSON. The version layer focuses on which fact was updated, which opinion had its confidence lowered, which theory was replaced and which evidence changed the conclusion.

In short:

```text
Markdown explains how to read.
JSON explains why we believe it.
Semantic Diff explains how the belief changed.
```

The version layer turns OIRF from a knowledge snapshot format into a knowledge evolution format.

## Core Entities

OIRF v0.1 uses three core reasoning entities:

- **Fact**: verifiable observations, data points or events;
- **Opinion**: interpretations, judgments, forecasts, recommendations or hypotheses;
- **Theory**: frameworks, models or mechanisms used to interpret facts and generate opinions.

It also keeps four supporting entities:

- **Source**: original materials, interviews, databases, files or AI outputs;
- **Evidence**: extracted proof units used to support or challenge conclusions;
- **Actor**: researchers, reviewers, organizations or AI systems;
- **Task**: research tasks, human-AI collaboration records and review workflows.

Recommended relationship:

```text
source -> evidence -> fact -> opinion -> report
                    theory -> opinion

actor -> task -> review -> semantic_diff
```

## Markdown Tag Example

Markdown is the presentation layer. It should remain natural to read, with only key statements linked to reasoning-layer IDs.

```md
In 2023, China's variable-frequency air-source heat pump market was approximately RMB 11.46 billion.  
[#fact:F-2023-vf-market-size] [#evidence:E-slide8-chart]

We judge that the market grew faster than the overall air-source category mainly because of rising variable-frequency penetration.  
[#opinion:O-penetration-growth] [#theory:T-penetration-upgrade]
```

## Reasoning JSON Example

```json
{
  "id": "fact:F-2023-vf-market-size",
  "type": "fact",
  "statement": "In 2023, China's variable-frequency air-source heat pump market was approximately RMB 11.46 billion.",
  "status": "reviewed",
  "measurement": {
    "metric": "market_size",
    "value": 11.46,
    "unit": "RMB billion",
    "period": "2023",
    "geography": "Mainland China",
    "segment": "variable-frequency air-source heat pump"
  },
  "evidenceIds": ["evidence:E-slide8-chart"],
  "sourceIds": ["source:S-air-source-ppt"],
  "ownerActorId": "actor:A-data-owner",
  "confidence": {
    "level": "medium",
    "rationale": "The number is visible in the report chart, but the original model and source data are not available."
  }
}
```

## Semantic Diff Example

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
      "reason": "New counter-evidence suggests actual 2024 sales may be lower than forecast."
    }
  ]
}
```

This diff does not merely say which line changed. It records which research judgment changed and why.

## Repository Contents

The repository is expected to include:

- `SPEC.md`: OIRF v0.1 draft specification;
- `TAGGING.md`: Markdown tagging rules;
- `schemas/reasoning.schema.json`: Reasoning Layer JSON Schema;
- `schemas/semantic-diff.schema.json`: Semantic Diff JSON Schema;
- `examples/`: a minimal example report package;
- `report/`: Markdown presentation-layer examples;
- `reasoning/`: facts, opinions, theories, sources, evidence, actors and tasks;
- `versions/`: semantic version-change examples.

Recommended structure:

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

## What OIRF Is Not

OIRF is not a report writing template. It does not prescribe how every report must be written.

OIRF is not a database product. It does not replace knowledge bases, BI tools, document systems or report production tools.

OIRF does not require all raw materials, interview transcripts, client files or AI conversations to be made public.

OIRF does not automatically guarantee that research conclusions are correct. It only makes conclusions easier to trace, inspect, reuse and update.

## Current Status

```text
Status: Draft / Proof of Concept
Version: v0.1
```

Current discussion topics:

- Whether the three-layer architecture is sufficient;
- Whether `fact / opinion / theory` is a stable core entity model;
- Whether Markdown tags fit real report writing workflows;
- Whether the Reasoning JSON Schema covers core industry research scenarios;
- Whether Semantic Diff can express changes in conclusions, evidence, assumptions and confidence;
- How OIRF should connect with JSON-LD, RDF, knowledge graphs and enterprise knowledge bases.

## Roadmap

- v0.1: Draft specification, schemas, tagging rules and minimal examples;
- v0.2: Real-world report examples and more industry scenarios;
- v0.3: JSON-LD / RDF mappings and knowledge graph export;
- v0.4: Basic validators and Markdown tag checkers;
- v1.0: Stable specification, community review and standardization discussion.

## Contributing

Contributions are welcome in the following areas:

- Schema design;
- Markdown tag syntax;
- Semantic diff rules;
- Industry report examples;
- JSON-LD / RDF mappings;
- Human-AI collaboration records;
- Evidence audit and permission management;
- Multilingual documentation.

Please use Issues, Pull Requests or Discussions to share suggestions.

## License Recommendation

Recommended licenses:

- Documentation: Creative Commons Attribution 4.0 International (CC BY 4.0)
- Schemas and example code: Apache License 2.0 or MIT License

If example reports are reconstructed from restricted materials, client materials or commercial reports, disclosure level and usage rights should be marked separately.
