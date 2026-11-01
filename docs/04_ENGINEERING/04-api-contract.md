# API contract draft — v1

All endpoints under `/api/v1`; authenticated internal access when deployed, session/CSRF protection where applicable. Return structured errors `{code,message,details,request_id}`.

| Method | Path | Responsibility |
|---|---|---|
| POST | /projects | Create named project |
| GET | /projects | List projects |
| GET | /projects/{id} | Project details and revisions |
| POST | /projects/{id}/sources | Upload validated photo / signed upload start |
| PATCH | /projects/{id}/crop | Save crop and canvas dimensions |
| POST | /projects/{id}/generations | Start generation; return job ID |
| GET | /generations/{run_id} | Poll status/result/reason |
| POST | /generations/{run_id}/cancel | Cancel when supported |
| GET | /revisions/{id} | Fetch revision geometry/palette/warnings |
| POST | /revisions/{id}/edits/merge | Merge adjacent regions into a new draft revision |
| POST | /revisions/{id}/validate | Run preflight validation |
| POST | /revisions/{id}/approve | Freeze validated revision |
| POST | /revisions/{id}/exports | Generate/reuse bundle for specified settings |
| GET | /exports/{id} | Export manifest and secure download links |
| GET | /paints | List available physical paint SKUs |

## Example generation request
```json
{"source_image_id":"UUID","canvas_mm":{"width":300,"height":400},"target_colour_count":24,"difficulty":"beginner","min_region_area_mm2":25,"semantic_protection":"auto"}
```

## Server guarantees
- Idempotency key on create-generation/create-export endpoints; repeated retries must not create duplicate jobs for same intent.
- Validate user authorization on every project/revision ID, never assume signed URL alone authorises editing.
- Upload preflight limits: preliminary 25 MB, dimensions and decompression-bomb validation; tune after prototype.
- Job progression only through defined states; unrecognised state is an error.
- Rate-limit costly generation, enforce job concurrency; do not block API request during inference.

## Error codes
INVALID_IMAGE, TOO_LARGE, BAD_DIMENSIONS, PROJECT_NOT_FOUND, ACCESS_DENIED, MODEL_UNAVAILABLE, GENERATION_FAILED, REVISION_NOT_VALIDATED, UNMAPPED_REGION, PAINT_SKU_UNAVAILABLE, EXPORT_FAILED.

## Future
Publish OpenAPI 3.1 YAML from validated Pydantic schemas; this document is a draft contract, not a checked schema.
