# System Topology Manifest (STM)

The **System Topology Manifest (STM)** is an open, graph-structured specification
for describing how software systems, services, and data flows interconnect.

STM defines deterministic metadata for system relationships — a machine-readable map
of each node’s triggers, inputs, and outputs.

It serves as the foundation for **Network-Augmented Generation (NAG)**,
and integrates naturally with **Knowledge-Augmented (KAG)** and **Retrieval-Augmented (RAG)** AI systems.

By embedding STM manifests across repositories, organizations can:
- Automatically visualize and reason over full system topologies
- Enable deterministic, AI-assisted orchestration
- Provide structured, verifiable context for retrieval and knowledge models

---

## Features

- **Graph-Structured Metadata:** Standard JSON/YAML format describing systems, triggers, inputs, and outputs.
- **AI-Assisted Generation:** Supported by the [Cognitive Scaffolding Protocol](ai/cognitive-scaffolding-protocol.md).
- **Compatibility:** Designed for use with RAG, KAG, and NAG-based AI pipelines.
- **Deterministic Validation:** All manifests are validated via [`stm-schema.json`](stm-schema.json).
- **Extensible:** Can be embedded in `package.json` or used standalone.

---

## Example STM (JSON)

```json
{
  "name": "inventory-sync",
  "description": "Pulls daily stock levels from ERP and updates storefront database",
  "appType": "worker",
  "technologyStack": "Node.js",
  "trigger": { "scheduler": "02:00" },
  "input": { "erp": "inventoryData" },
  "output": { "sql": "productInventory", "grafana": "metrics" }
}
```

---

## Repository Structure

```
/
├─ README.md                     # Specification and overview
├─ stm-schema.json               # JSON Schema definition
│
├─ examples/
│  ├─ stm.example.json           # Full reference example
│  ├─ stm.scaffold.json          # Starter template
│  └─ scaffolding-guide.md       # Human-readable scaffolding guide
│
├─ ai/
│  └─ cognitive-scaffolding-protocol.md   # AI/human reasoning guide for STM generation
│
├─ tools/
│  └─ stm-scaffolder.js          # Optional CLI scaffolder
│
└─ LICENSE
```

---

## Validation

To validate an STM file against the schema:

```bash
npx ajv validate -s stm-schema.json -d examples/stm.example.json
```

---

## Quick Start

1. Add an STM block to your project (`package.json` or `.stm` file).  
2. Validate it using the included schema or scaffolder tool.  
3. Push to your repository — automated hooks can register STM manifests to your system graph.

---

## Topics

`ai`, `rag`, `kag`, `nag`, `system-topology`, `metadata-schema`, `knowledge-graph`,  
`graph-structured-data`, `deterministic-ai`, `ai-orchestration`, `workflow-automation`,  
`ontology`, `data-topology`, `open-standard`, `semantic-schema`, `machine-readable`,  
`ai-scaffolding`, `cognitive-scaffolding`, `infrastructure-graph`, `system-architecture`

---

## License

```
Copyright (c) 2025 SystemTopologyManifest
Released under the MIT License.
```

---

**System Topology Manifest (STM)** — a graph-structured metadata standard for  
**Network-, Knowledge-, and Retrieval-Augmented AI systems.**
