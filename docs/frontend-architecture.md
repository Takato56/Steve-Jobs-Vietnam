# Frontend Architecture

The frontend lives in the same repository for hackathon delivery but is a separate deployable application. It does not import Python modules, inspect `data2/`, or access Chroma directly.

```mermaid
flowchart LR
    Citizen[Citizen] --> Portal[Next.js portal demo]
    Portal --> Assistant[Embedded GovEase assistant]
    External[Existing public-service portal] -->|embed.js + iframe| Widget[/widget]
    Widget --> Assistant
    Assistant -->|HTTPS JSON /api/v1| API[FastAPI on Render]
    API --> Intake[Guided intake]
    API --> Forms[Procedure form schema]
    API --> Rules[Validation engine]
    API --> RAG[Chroma + OpenAI]
    RAG --> Sources[dichvucong.gov.vn citations]
```

## UI composition

- `PortalHeader`: simulated public-service navigation and explicit demo marking.
- `CitizenAssistant`: state machine for need, guidance and validation.
- `GuidanceView`: documents, conditional requirements, steps and citations.
- `DynamicForm`: renders controls from `form-schema`, creates nested submissions and maps issues to fields.
- `FloatingAssistant`: demonstrates an in-portal chatbot entry point.
- `/widget` and `public/embed.js`: iframe integration for a plain external website.

## Models and APIs

The browser never calls OpenAI or Chroma. It uses the backend endpoints documented in `api-integration.md`. The backend uses `text-embedding-3-small` for retrieval, a configurable Codex model through the Responses API for generated answers, Chroma for vector search, and deterministic plus optional semantic validation.

## State and privacy

Conversation state is returned as `session_id`; the browser sends up to the 12 most recent user/assistant messages with the next request. Chat history, the suggested procedure, related procedures and the current clarification question are stored in `localStorage` so the floating assistant and main chat share one conversation. Clearing the conversation removes that entry.

Form values remain in React memory and are sent only to `/validate`. Identity numbers and form submissions are not written to browser storage. The backend must not log submission bodies, identity numbers, addresses or document contents.

## Deployment

- Frontend: Vercel with root `vercel.json` and `NEXT_PUBLIC_API_URL` pointing to Render.
- Backend: Render with the Vercel origin included in `CORS_ORIGINS`.
- `/widget` can be embedded on any allowed origin through `embed.js`.
