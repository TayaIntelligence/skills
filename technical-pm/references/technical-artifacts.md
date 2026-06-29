# Technical Artifacts Reference

The "technical" half of the technical-PM job: the engineering-facing scaffolding a PRD needs so a
team can actually build from it. Output language follows the user (中文 by default);
write plainly and avoid pairing terms with English — keep a term only when it's genuinely standard,
glossed once.

> **Why this exists.** A PRD that says "AI fills in the fields" or "the system syncs the data" is not
> buildable — it hides every decision an engineer has to make. Where a PM earns the word *technical*
> is in pinning down the **contracts** (what goes in, what comes out), the **error behavior** (what
> happens when it doesn't work), and the **non-functional bar** (how fast / safe / reliable). Pull
> from this when a feature has an interface, moves data between systems, or calls a service.

## Contents
- Recommendation vs. action (who owns side effects)
- Interface / API contracts
- Data contracts
- Error taxonomy (system failure vs. business warning)
- Non-functional requirements as testable AC
- Boundary & edge-case checklist

---

## Recommendation vs. action — separate them
A common, powerful architecture for risky automation: **the smart component only proposes; a
deterministic layer executes the side effects** (moving files, writing records, sending money). Spell
out which side owns what. Benefits: the risky part stays reversible and reviewable, side effects are
auditable, and a human (or rules engine) can sit in between. In the PRD, state plainly: *"Service X
returns a structured suggestion + reason; engineering layer Y validates and performs the write."*

## Interface / API contracts
For each endpoint the feature needs, specify **input**, **output**, and an **example payload**. Tables
beat prose — they force you to name every field, type, and whether it's required.

**Input** — columns: `字段 field` · `类型 type` · `必填 required` · `说明 notes (source, format, enum values)`.
**Output** — same shape, plus who produces each field (see data contracts).

Always include a concrete example payload (real-looking JSON), and for each field note **format**
(e.g. date = `YYYYMMDD`), **enum** (the allowed values), and **nullability** (when is it `null`, and
what does `null` mean — "not applicable" vs. "couldn't determine"). Ambiguous `null` semantics are a
top source of integration bugs.

## Data contracts
When data crosses a system boundary, agree on the schema *before* coding:
- **Field ownership** — for every field, who is the source of truth? (e.g. some fields produced by the
  AI service, others auto-filled by the host system.) A simple "输出方 / owner" column removes weeks of
  "I thought you were setting that."
- **Identifiers** — what ID ties the request to the response? Is it echoed back verbatim?
- **Formats & units** — dates, currencies, locales, encodings. State them once, explicitly.
- **Versioning** — how does the contract evolve without breaking callers (additive fields, version
  field, deprecation path)?

## Error taxonomy — system failure vs. business warning
The single most-skipped part of a PRD, and the one engineers most need. Split failures into two kinds,
because they're handled completely differently:

1. **System-level error** — the service *cannot complete* the call. Return a non-2xx HTTP status, an
   `error_code`, and no business fields. The caller must fall back (retry, queue for manual, alert).
   *Examples:* unsupported format (422), encrypted/locked input (422), timeout/oversized (504),
   internal error (500).
2. **Business-level warning** — the call *succeeds* (HTTP 200) but some result is low-confidence or a
   rule is missing. Return the usable fields, set a `needs_review = true` flag, and list machine-readable
   `review_reasons` / `warning_code`s. The caller routes to a human but keeps the partial result.

Capture each in a table: `code` · `trigger` · `service behavior` · `downstream/engineering handling`.
*Why the split matters:* conflating them means either the system hard-fails on recoverable issues, or
silently archives bad data. Naming the two lanes makes the human-in-the-loop design fall out naturally.

## Non-functional requirements (NFR) as testable AC
"Fast", "reliable", "secure" are not requirements — they're untestable wishes. Convert each into a
number a tester can check:
- **Latency** → "p95 < 500ms" (pick the percentile, not the average — averages hide tail pain).
- **Throughput / size limits** → "handles files up to 100MB; rejects larger with a clear error."
- **Timeout** → "single call times out at 30s and returns `TIMEOUT`."
- **Availability** → "99.5% monthly", and what the client does during the 0.5%.
- **Security / compliance** → name the regime (audit trail, data residency, PII handling) and the
  observable check, not just "compliant".

Write these as acceptance criteria so they're verified, not assumed. An NFR with no number is a
deferred argument.

## Boundary & edge-case checklist
Run every input-handling feature through this list and decide the behavior (handle / reject / degrade)
*before* build — these are where production breaks:
- Oversized / empty / zero-byte input
- Encrypted, password-protected, or corrupted files
- Malformed encoding, wrong format, mixed languages
- Missing required upstream data (the field the rule depends on isn't there)
- Duplicate / retried requests (idempotency)
- Partial success (some fields extracted, others not)

Naming these in the PRD turns "the AI/service handles it somehow" into explicit, reviewable decisions.
