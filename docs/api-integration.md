# API Integration Guide

The external frontend should set its API base URL to the deployed Render service and call `/api/v1` endpoints. The complete interactive contract is available at `/docs` and `/openapi.json`.

## Context-aware chatbot

Use `/chat` for the conversational UI. It returns display-ready structured fields in addition to the plain `answer` fallback.

```http
POST /api/v1/chat
Content-Type: application/json
```

```json
{
  "message": "Tôi cần làm giấy tờ cho con mới sinh",
  "history": [],
  "active_procedure_code": null
}
```

The response contains:

- `summary`, `key_points` and `next_step` for structured rendering;
- `procedure` only when the context is sufficiently clear;
- up to three `related_procedures` for broad requests;
- at most one `clarifying_question` in a response;
- `sources` whose `source_url` links to the corresponding official procedure detail page;
- `mode`, which lets the UI distinguish clarification, grounded guidance and fallback responses.

Send no more than the 12 most recent user/assistant messages in `history`. Do not set `active_procedure_code` until a procedure is actually selected. If the user explicitly changes topic, send the new message with the recent history and let the backend re-evaluate the context instead of forcing the previous procedure code.

## Guided intake

```http
POST /api/v1/intake
Content-Type: application/json
```

```json
{
  "message": "Tôi mới sinh con và cần đăng ký khai sinh",
  "history": [],
  "answers": {}
}
```

Keep the returned `session_id`. When the response has `status=needs_clarification`, render only the single returned `clarifying_question`; do not render a selected procedure, checklist or official sources yet. Append the question and the user's answer to `history` on the next request. Once the frontend knows the chosen procedure, it may send `procedure_code` to keep guided intake scoped.

## Procedure catalog

```http
GET /api/v1/procedures
GET /api/v1/procedures/1.001193
```

Use the catalog for browsing and the detail endpoint for procedure metadata. Do not infer validation depth from catalog presence: all 20 reviewed records support grounded guidance, while detailed field validation currently covers `1.001193` and `1.004194`.

## Build a form

```http
GET /api/v1/procedures/1.001193/form-schema
```

Render controls from `fields`. Use `path` as the stable field identifier and send the resulting nested object to:

```http
POST /api/v1/procedures/1.001193/validate
```

```json
{
  "submission": {
    "child": {"full_name": "Nguyễn Minh An", "birth_date": "2026-07-01", "gender": "Nam"},
    "mother": {"identity_number": "079204001234"},
    "requester": {"identity_number": "079204001235", "relationship_to_child": "Mẹ"},
    "birth_certificate": {"available": true, "birth_date": "2026-07-01"},
    "submission": {"signature_or_confirmation": true}
  }
}
```

Map every returned issue to its `field`. `blocking=true` prevents submission; warnings should remain visible but need not block the user.

## Administrative address options

Administrative-address fields are cascading controls. Fetch each level only after its parent has been selected:

```http
GET /api/v1/administrative-units/provinces
GET /api/v1/administrative-units/districts?province_code=01
GET /api/v1/administrative-units/wards?province_code=01&district_code=001
```

Each endpoint returns `items` containing `{ "code": "...", "name": "..." }`. Store both code and display name in the nested address object and collect the house number/street/alley or village/group in `detail`. Treat upstream failures as recoverable UI errors; do not silently accept arbitrary administrative-unit text as an official selection.

## Stable errors

```json
{
  "error": {
    "code": "PROCEDURE_NOT_FOUND",
    "message": "Không tìm thấy thủ tục.",
    "request_id": "..."
  }
}
```

Send an optional `X-Request-ID` header to correlate frontend and backend logs. Configure the frontend origin in the backend `CORS_ORIGINS` allowlist.
