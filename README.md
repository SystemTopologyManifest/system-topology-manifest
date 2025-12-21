# System Topology Manifest (STM)

The **System Topology Manifest (STM)** is an open, graph-structured specification for describing how software systems, services, and data flows interconnect. STM records declared nodes and signals; graph edges may be derived downstream by consuming systems.

STM defines deterministic metadata for system relationships — a machine-readable map of each node’s triggers, inputs, and outputs.

It serves as the foundation for **Network-Augmented Generation (NAG)**, and integrates naturally with **Knowledge-Augmented (KAG)** and **Retrieval-Augmented (RAG)** AI systems.

By embedding STM manifests across repositories, organizations can:
- Automatically visualize and reason over full system topologies
- Enable deterministic reasoning and analysis over system topology
- Provide structured, verifiable context for retrieval and knowledge models

## Canonical Definition

The System Topology Manifest (STM) is a declarative, machine-readable description of
systems, applications, and the signals they emit or consume.

STM defines *what exists* and *what communicates*.

STM does not define architecture, policy, constraints, or enforcement.
Those are derived downstream by consuming systems such as the
System Architecture Manifest (SAM).

### What STM Does Not Define

STM is strictly descriptive.

It does not define:
- architecture principles
- guardrails or enforcement rules
- coding standards or DevOps policy
- deployment behavior or runtime control

Those concerns are handled by the System Architecture Manifest (SAM).

### Features

- **Graph-Structured Metadata:** Standard JSON/YAML format describing systems, triggers, inputs, and outputs.
- **AI-Assisted Generation:** Supported by the [Cognitive Scaffolding Protocol](ai/cognitive-scaffolding-protocol.md).
- **AI-Assisted Generation for package.json:** Supported by the [Cognitive Scaffolding Protocol - package.json](ai/cognitive-scaffolding-protocol.packagejson.md).- **Compatibility:** Designed for use with RAG, KAG, and NAG-based AI pipelines.
- **Deterministic Validation:** All manifests are validated via [`stm-schema.json`](stm-schema.json).
- **Extensible:** Can be embedded in `package.json` or used standalone.

### Example STM (JSON)

STM definitions may appear in two forms:

1. Canonical STM Files
	-	stm.example.json
	-	stm.scaffold.json
	-	Generated STM outputs consumed by SAM

These use the canonical structure:
```json
{
  "schema_version": "1.0",
  "generated": "...",
  "apps": [
    {
      "id": "inventory-sync",
      "name": "Inventory SYNC",
      "description": "Pulls daily stock levels from ERP and updates storefront database",
      "appType": "worker",
      "technologyStack": "Node.js",
      "trigger": { "scheduler": "02:00" },
      "input": { "erp": "inventoryData" },
      "output": { "sql": "productInventory", "grafana": "metrics" }
    },
    { ... }
  ]
}
```

2. Embedded STM Fragments (package.json)

For local, AI-assisted, or repo-scoped documentation, STM entries may be embedded directly in package.json under the stm key:
```json
"stm": [
  {
    "id": "<kebab-case-name>",
    "name": "<Human-readable name>",
    "description": "<What this system does>",
    "triggers": [],
    "inputs": [],
    "outputs": [],
    "confidence": 1.0
  }
]
```

If a repository defines **only one system**, the `stm` value MAY be a single object
instead of an array.

if items are already defined in the root properties of the package.json file placeholders may be used:
```json
"stm": {
 "id": "[name]",
 "name": "<Human-readable name>",
 "description": "[description]",
 "triggers": [],
 "inputs": [],
 "outputs": [],
 "confidence": 1.0
}
```

**Notes**
-	This format avoids collisions with existing keys (e.g. apps used by PM2).
-	Embedded STM fragments are non-authoritative and may be partial.
-	The confidence field is optional metadata indicating author or AI certainty.
-	When producing canonical STM artifacts, tooling may:
  -	normalize stm[] → apps[]
  -	preserve or discard confidence as non-normative metadata

### Repository Structure

```
/
├─ README.md                     # Specification and overview
├─ stm-schema.json               # JSON Schema definition
│
├─ examples/
│  ├─ stm.example.json           # Full reference example
│  └─ stm.scaffold.json          # Starter template
│
├─ ai/
│  ├─ cognitive-scaffolding-protocol.md               # AI/human reasoning guide for STM generation
│  └─ cognitive-scaffolding-protocol.packagejson.md   # AI/human reasoning guide for package.json STM generation
│
└─ LICENSE
```

### Validation

To validate an STM file against the schema:

```bash
# validate canonical STM file
npx ajv validate -s stm-schema.json -d examples/stm.example.json

# validate embedded stm fragment inside package.json
jq '.stm' package.json > /tmp/stm.fragment.json
npx ajv validate -s stm-schema.json -d /tmp/stm.fragment.json
```

### Quick Start

1. Add an STM block to your project (`package.json` or `.stm` file).  
2. Validate it using the included schema or scaffolder tool.  
3. Push to your repository — automated hooks can register STM manifests to your system graph.

### Related Projects

- **System Architecture Manifest (SAM)**  
  Consumes STM output to derive architecture guardrails, constraints, and policies.  
  https://github.com/SystemArchitectureManifest/system-architecture-manifest

### Topics

`ai`, `rag`, `kag`, `nag`, `system-topology`, `metadata-schema`, `knowledge-graph`,  
`graph-structured-data`, `deterministic-ai`, `ai-orchestration`, `workflow-automation`,  
`ontology`, `data-topology`, `open-standard`, `semantic-schema`, `machine-readable`,  
`ai-scaffolding`, `cognitive-scaffolding`, `infrastructure-graph`, `system-topology`

### Contributing

We welcome contributions to expand and improve the System Architecture Manifest project. If you’re interested, please check out the **[contributing guidelines](./CONTRIBUTING.md)** for more information.

### Contact

- **Author:** William Shostak (https://github.com/wshostak)

### License

This project is licensed under the **ISC License** — see the **[LICENSE](./LICENSE.txt)** file for more details.

Copyright (c) 2025 William Shostak

**System Topology Manifest (STM)** — a graph-structured metadata standard for  
**Network-, Knowledge-, and Retrieval-Augmented AI systems.**
