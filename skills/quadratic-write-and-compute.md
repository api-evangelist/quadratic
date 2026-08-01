---
name: write-quadratic-cells-and-code
description: Write values and run Python/SQL/formula code cells in a Quadratic file via the Developer API.
api: Quadratic Developer API
base_url: https://developer-api.quadratichq.com
auth: Authorization; Bearer qdx_live_… (or qdx_test_…)
operations:
- createFile
- addSheet
- setCellValues
- setFormulaCells
- setSqlCell
- setCodeCell
- rerunCode
- batch
- undo
generated: '2026-07-20'
method: generated
source: openapi/quadratic-openapi.json
---

# Write cells and run code in Quadratic

Grounded in real operationIds from `openapi/quadratic-openapi.json`. All calls send
`Authorization: Bearer <qdx_live_… token>`. Prefer `qdx_test_…` tokens while developing
(`sandbox/quadratic-sandbox.yml`).

## Steps

1. **Create or pick a file.** `createFile` (`POST /v1/files`, needs `team_uuid`) or reuse a
   `file_id`. Add tabs with `addSheet` (`POST /v1/files/{file_id}/sheets`).
2. **Write values.** `setCellValues` (`PUT /v1/files/{file_id}/cells`) with
   `sheet_name`, `top_left_position`, and a 2-D `values` array.
3. **Compute.** Insert logic with `setFormulaCells` (`PUT .../cells/formula`),
   `setSqlCell` (`PUT .../cells/sql`, references a Connection), or `setCodeCell`
   (`PUT .../cells/code` for Python/JavaScript). Re-execute with `rerunCode`
   (`POST .../cells/code/rerun`).
4. **Group and reverse safely.** Wrap multi-step edits with `batch`
   (`POST /v1/files/{file_id}/batch`) for atomicity; recover with `undo`/`redo`.

## Conventions

- No `Idempotency-Key` header — use `batch` + `undo`/`redo` for safe/reversible writes
  (`conventions/quadratic-conventions.yml`).
- Errors return `{ "error": { "code", "message" } }`; a `502` typically signals a
  compute/connection backend failure (`errors/quadratic-problem-types.yml`).
