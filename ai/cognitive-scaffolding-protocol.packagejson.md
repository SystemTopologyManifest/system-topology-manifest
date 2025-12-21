# Generating STM Sections for `package.json` Files
**Purpose:** Help AI tools (Copilot, ChatGPT, Windsurf, etc.) generate consistent `stm` blocks for `package.json`.

## Overview
This guide tells an AI how to create or update the `stm` block in a `package.json`.
The output should be a **first-draft**, syntactically correct definition of the app(s) in the package.

- This is **package-local metadata** (repo-scoped).
- It may be incomplete.
- It is intended to be normalized into canonical STM artifacts later.

### Structure

If the package defines **one app**, create:

```json
"stm": { ... }
```

If it defines **multiple apps**, create:

```json
"stm": [ { ... }, { ... } ]
```

#### Required shape (per app)

```json
{
  "id": "app:<unique-id>",
  "name": "[name]",
  "description": "[description]",
  "appType": "",
  "technologyStack": "",
  "triggers": [],
  "inputs": [],
  "outputs": [],
  "confidence": 1.0
}
```

##### Field notes
- `id`: stable unique identifier. Prefer `app:<kebab-case>` or a UUID-based suffix if collisions are possible.
- `name`: human-readable name (can mirror `package.json.name`).
- `description`: verb-first, plain English.
- `appType`: e.g. `worker`, `api`, `router`, `frontend`, `etl`, `module`, `bot`.
- `technologyStack`: short string (e.g. `"Node.js"`, `"Node.js + Express"`).
- `triggers/inputs/outputs`: arrays of short strings. Keep them small and factual.
- `confidence`: optional number in `[0..1]` indicating author/AI certainty (used to prioritize review; not “truth”).

## Generation Rules (for AI)

1. **Placeholders**
   Keep `[name]` and `[description]` when the same data already exists elsewhere in the package.
   These are resolved automatically later—do not replace them.

2. **appType inference**
   Use heuristics:
   - Has `/flows` → `etl`
   - Uses Express or `/routes` → `router`
   - Uses Petite-Vue/Vue/React → `frontend`
   - Mentions “bot”, “automation”, or cron/scheduler → `bot` (or `worker` if clearly job-runner)
   - Otherwise → `module`

3. **technologyStack inference**
   Derive from dependencies:
   - Node.js, Express, Node-RED, Axios, Mongoose, Kantan Logger, etc.
   - Include internal libs like `log-to-db`, `node-config`, `node-utilities` when present.
   Keep it short (1–3 key components).

4. **triggers**
   Triggers describe **what starts work** (not what it does).
   Use short strings like:
   - `"webhook: acumatica salesorder created"`
   - `"scheduler: nightly 02:00"`
   - `"user: dashboard action"`

5. **inputs**
   Inputs describe **what is read** (systems + dataset).
   Use short strings like:
   - `"zendesk: ticket fields + side comments"`
   - `"db: config documents"`
   - `"erp: inventoryData"`

6. **outputs**
   Outputs describe **what is written/affected**.
   Use short strings like:
   - `"acumatica: company record updates"`
   - `"sql: productInventory"`
   - `"grafana: metrics"`

7. **Style**
   - Concise, plain English.
   - Start descriptions with verbs (“Processes”, “Integrates”, “Generates”).
   - Prefer lowercase system names inside the strings.
   - Produce valid JSON for `package.json` (minified is fine).

### Completion Checklist

Before returning a `stm` block, ensure:
- `appType` is inferred and realistic
- At least one of `triggers`, `inputs`, or `outputs` is non-empty
- `technologyStack` lists key frameworks/libraries
- Placeholders remain where package data exists
- `description` is short, clear, and factual
- `confidence` reflects certainty (lower if inferred)

### Example Instruction (for AI)

Task:
Generate or update a `stm` section for this package based on its existing `package.json`.
Use placeholders for `name` and `description` when those values exist elsewhere.
Infer `appType`, derive `technologyStack` from dependencies,
and populate `triggers`, `inputs`, and `outputs` with short factual strings.

### Example Output

```json
"stm": {
  "id": "app:tigger",
  "name": "[name]",
  "description": "[description]",
  "appType": "router",
  "technologyStack": "Node.js + Express",
  "triggers": ["webhook: incoming events"],
  "inputs": ["db: queue state + routing rules"],
  "outputs": ["webhook: forwarded JSON events", "db: execution logs"],
  "confidence": 0.85
}
```
