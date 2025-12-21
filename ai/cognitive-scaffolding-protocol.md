# Generating `stm.manifest.json`
**Purpose:** Help AI tools generate a canonical **System Topology Manifest** file (`stm.manifest.json`) from repo-scoped facts (e.g., `package.json` `stm`, docs, and known integrations).

## Overview
`stm.manifest.json` is the **canonical, repo-level STM artifact**.

- It is **descriptive** (what exists, what it touches).
- It is **deterministic** (no probabilistic fields required).
- It does **not** define topology edges (those are derived later when building SAM).

### Structure

Canonical STM manifest shape:

```json
{
  "schema_version": "1.0",
  "generated": "2025-01-01T00:00:00.000Z",
  "apps": [ { ... }, { ... } ]
}
```

#### Required shape (per app)

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

##### Notes
- `apps` is **always an array** in the canonical manifest (even if only one app).
- `trigger`, `input`, `output` are **objects** whose values are short strings.
- Keep system keys **lowercase** (e.g., `erp`, `zendesk`, `acumatica`, `sharepoint`, `sql`, `mongodb`, `http`, `email`).

### Sources of truth (priority order)

1. **Repo-local STM fragments** in `package.json` (`stm` key)
2. Existing repo docs (README, `/docs`, `.ai-guidelines`, etc.)
3. Dependency signals (runtime/framework inference)
4. Known integration modules / adapters referenced by name

If a fact is not supported by any source, leave it out or keep it generic.

### Normalization Rules (for AI)

1. **From `package.json.stm` → `apps[]`**
   - If `package.json.stm` is an object, normalize to a 1-item array.
   - Map fields if present:
     - `triggers[]` → `trigger{}` (see rule 4)
     - `inputs[]` → `input{}`
     - `outputs[]` → `output{}`
   - Drop package-only metadata like `confidence` unless explicitly requested.

2. **App naming**
   - `name` should be stable, machine-friendly (kebab-case is preferred).
   - `description` should be short, verb-first, factual.

3. **`appType`**
   Use a small, consistent set: `worker`, `api`, `router`, `frontend`, `etl`, `module`, `bot`.
   Prefer the most specific label you can justify from repo evidence.

4. **Convert arrays to objects (when needed)**
   Canonical STM uses objects for `trigger/input/output`.

   - If you only have strings like `"scheduler: nightly 02:00"`:
     - convert to `{ "scheduler": "nightly 02:00" }`
   - If you have `"webhook: acumatica salesorder created"`:
     - convert to `{ "webhook": "acumatica salesorder created" }`
   - If you have `"sql: productInventory"`:
     - convert to `{ "sql": "productInventory" }`

   Keep keys low-cardinality and recognizable (`scheduler`, `webhook`, `user`, `cron`, `email`, etc.).

5. **`technologyStack`**
   Keep it short (1–3 key components):
   - `"Node.js"`
   - `"Node.js + Express"`
   - `"Node-RED + Node.js"`

6. **No topology**
   Do not create `graph_edges` here.
   Do not infer read/write directionality beyond labeling `input` vs `output`.

### Style Constraints

- Use concise, plain English.
- Avoid speculative claims.
- Prefer fewer, higher-confidence entries over exhaustive guesses.
- Maintain consistent system keys across apps (e.g., always `sql`, not sometimes `mssql`).

### Completion Checklist

Before returning `stm.manifest.json`, ensure:
- Top-level contains `schema_version`, `generated`, and `apps`
- `apps` is always an array
- Each app has: `name`, `description`, `appType`, `technologyStack`, `trigger`, `input`, `output`
- `trigger/input/output` values are short strings
- Keys are lowercase and consistent
- No `graph_edges` or guardrail content is included

### Example `stm.manifest.json`

```json
{
  "schema_version": "1.0",
  "generated": "2025-01-01T00:00:00.000Z",
  "apps": [
    {
      "name": "inventory-sync",
      "description": "Pulls daily stock levels from ERP and updates storefront database",
      "appType": "worker",
      "technologyStack": "Node.js",
      "trigger": { "scheduler": "02:00" },
      "input": { "erp": "inventoryData" },
      "output": { "sql": "productInventory", "grafana": "metrics" }
    },
    {
      "name": "order-webhook-router",
      "description": "Routes incoming order events to internal services for downstream processing",
      "appType": "router",
      "technologyStack": "Node.js + Express",
      "trigger": { "webhook": "incoming order events" },
      "input": { "http": "webhook payload" },
      "output": { "queue": "enqueued jobs", "db": "execution logs" }
    }
  ]
}
```
