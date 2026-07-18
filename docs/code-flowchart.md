# GovEase-AI Code Flowchart

Sơ đồ này mô tả luồng hoạt động đúng theo code hiện tại. Nó tách rõ luồng hội thoại/hướng dẫn và luồng kiểm tra hồ sơ.

```mermaid
flowchart TD
    citizen[Người dân] --> ui[Next.js portal / widget] 
    ui --> api[FastAPI /api/v1]


    subgraph chatFlow["Luồng chat tạo hướng dẫn"]
        api --> chatEndpoint["POST /chat"]
        chatEndpoint --> resolveIntent["ChatService._resolve_intent()"]
        resolveIntent --> probe["ProcedureAssistant.guided_intake() probe"]
        probe --> intentDecision{"Đủ rõ thủ tục chưa?"}

        intentDecision -->|Chưa rõ| clarification["Trả về 1 câu hỏi làm rõ"]
        clarification --> ui

        intentDecision -->|Đủ rõ| scopedIntake["guided_intake() với procedure_code nếu có"]
        scopedIntake --> localClassifier["KeywordRetriever classify/search\nfew-shot + keyword fallback"]
        localClassifier --> procedureStore[(Procedure JSON trong data2)]
        procedureStore --> checklist["Checklist, bước, giấy tờ, nguồn"]

        scopedIntake --> retrievalQuery["Tạo retrieval_query\nmessage + title nếu đã resolve"]
        retrievalQuery --> chromaSearch["ChromaRetriever.search()"]
        chromaSearch --> embed["OpenAIEmbeddingService.embed_query()"]
        embed --> chroma[(Chroma vector collection)]
        chroma --> chunks["Retrieved chunks"]

        chunks --> hasChunks{"Có chunks + OpenAI key?"}
        checklist --> hasChunks
        hasChunks -->|Không| fallbackAnswer["Trả lời bằng local data / retrieval-only"]
        hasChunks -->|Có| llmAnswer["OpenAI Responses API\nsinh trả lời có cấu trúc"]
        fallbackAnswer --> chatResponse["ChatResponse: answer, summary,\nkey_points, sources, procedure, mode"]
        llmAnswer --> chatResponse
        chatResponse --> ui
    end
```

Các điểm cố ý không vẽ như module riêng:

- Không có stage `Reranker` riêng trong code hiện tại.
- Không có stage `Metadata Filter` runtime riêng trong code hiện tại.
- `AI Orchestrator` trong flow cũ được thay bằng các hàm/thành phần thật: `ChatService`, `ProcedureAssistant`, `ChromaRetriever`, `SubmissionValidator`.
