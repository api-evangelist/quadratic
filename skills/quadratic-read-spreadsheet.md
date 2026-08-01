---
name: read-quadratic-spreadsheet
description: Explore and read data from a Quadratic spreadsheet file via the Developer API.
api: Quadratic Developer API
base_url: https://developer-api.quadratichq.com
auth: Authorization; Bearer qdx_live_… (or qdx_test_…)
operations:
- listFiles
- getFile
- getContext
- getOutline
- listSheets
- getCells
- readData
- textSearch
generated: '2026-07-20'
method: generated
source: openapi/quadratic-openapi.json
---

# Read a Quadratic spreadsheet

Grounded in real operationIds from `openapi/quadratic-openapi.json`. All calls send
`Authorization: Bearer <qdx_live_… token>`.

## Steps

1. **Find the file.** Call `listFiles` (`GET /v1/files`, paginate with `page`) or start
   from a known `file_id`.
2. **Orient before reading.** Call `getContext` (`GET /v1/files/{file_id}/context`) for an
   LLM-ready summary of structure, then `getOutline` (`GET /v1/files/{file_id}/outline`)
   and `listSheets` (`GET /v1/files/{file_id}/sheets`) to enumerate tabs.
3. **Read cell ranges.** Call `getCells` (`GET /v1/files/{file_id}/cells`) with a
   `selection` and `sheet_name`; bound large reads with `max_rows` and `page`. Use
   `readData` (`GET /v1/files/{file_id}/data`) for structured data extraction.
4. **Search.** Call `textSearch` (`GET /v1/files/{file_id}/search`) with `query`,
   optionally `case_sensitive`, `regex`, `whole_cell`, or `search_code`.

## Conventions

- Cells are addressed by A1-style `selection`/`position` plus `sheet_name`, not ids
  (`conventions/quadratic-conventions.yml`).
- Errors return `{ "error": { "code", "message" } }`; switch on `error.code`
  (`errors/quadratic-problem-types.yml`).
