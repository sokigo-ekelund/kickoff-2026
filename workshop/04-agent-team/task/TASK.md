# The Task · Checklist Validator (CLI)

Small enough to finish, big enough to justify division of labor.

## The order (paste into Claude Code)

**Step 1 – baseline (no agents):**

```
Build a CLI tool in Node.js (one file, validator.js, no dependencies) that
validates JSON files containing playground inspection results according to
the rules below. Usage: node validator.js <file.json>. Output: every rule
violation with a reference, exit code 0 on OK and 1 on errors. Test it
against sample-inspection.json in this folder.
```

**Step 2 – through the team (same order, fresh folder or `git checkout .`):**

```
Solve the task below by delegating: have the planner produce the plan, the
implementer build according to the plan, the reviewer review the result,
and the implementer fix any critical findings. Finish by having the
doc-writer write a README. Do none of the work yourself in the main thread.

[same order as in step 1]
```

## Validation rules (data model from a real PRD)

An inspection file is a JSON object with:

- `playground` (string, required, non-empty)
- `type` – one of: `"visual"`, `"operational"`, `"annual_main"`
- `inspector` (string, required)
- `date` (ISO 8601, must not be in the future)
- `items` – array with at least 1 object: `{ "title": string, "answer": "ok" | "deviation" | "not_applicable" }`
- `deviations` – array (may be empty) of objects: `{ "description": non-empty string, "risk_class": "A" | "B" | "C", "item_index": integer pointing at an item in items }`

Cross-rules:
1. Every item with `answer: "deviation"` MUST have at least one deviation whose `item_index` points at it.
2. Every deviation MUST point at an item whose answer is `"deviation"`.
3. If any deviation has risk class `"A"`, the top-level field `action_started` (boolean) must exist.
4. `signature` (object with `name` and `timestamp`) is required if the inspection has `status: "done"`.

`sample-inspection.json` in this folder contains **deliberate errors** – the validator must find all of them.

## Answer key (for self-checking – look after the run)

The sample file violates: future date, one deviation points at an item answered "ok" (rule 2), one item answered "deviation" has no linked deviation (rule 1), risk class A without `action_started` (rule 3), and `status: "done"` without a signature (rule 4). Five errors in total.
