# GovEase-AI Code Flowchart

Sơ đồ này mô tả luồng hoạt động đúng theo code hiện tại. Nó tách rõ luồng hội thoại/hướng dẫn và luồng kiểm tra hồ sơ.

```mermaid
flowchart TD
    citizen[Người dân] --> ui[Next.js portal / widget]
    ui --> api[FastAPI /api/v1]

    api --> chatEndpoint["POST /chat"]
    api --> intakeEndpoint["POST /intake"]
    api --> catalogEndpoint["GET /procedures và /form-schema"]
    api --> validateEndpoint["POST /procedures/{code}/validate"]

    subgraph chatFlow["Luồng chat tạo hướng dẫn"]
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

    subgraph intakeFlow["Luồng guided intake API"]
        intakeEndpoint --> intakeCall["ProcedureAssistant.guided_intake()"]
        intakeCall --> intakeDecision{"needs_clarification?"}
        intakeDecision -->|Có| intakeClarify["IntakeResponse status=needs_clarification\nkhông trả selected checklist"]
        intakeDecision -->|Không| intakeDone["IntakeResponse status=completed\nprocedure + checklist + sources"]
        intakeClarify --> ui
        intakeDone --> ui
    end

    subgraph formFlow["Luồng tạo form và kiểm tra hồ sơ"]
        catalogEndpoint --> formSchema["services.procedures.form_schema()"]
        formSchema --> renderForm["Frontend render form fields"]
        renderForm --> citizenSubmission["Người dân nhập hồ sơ"]
        citizenSubmission --> validateEndpoint

        validateEndpoint --> checkSubmission["ProcedureAssistant.check_submission()"]
        checkSubmission --> ruleEngine["SubmissionValidator.validate()"]
        ruleEngine --> required["Required fields"]
        ruleEngine --> identity["Identity number format"]
        ruleEngine --> dateCheck["Date checks"]
        ruleEngine --> signature["Signature check"]
        ruleEngine --> procedureRules["Procedure-specific rules\nkhai sinh / tạm trú"]

        checkSubmission --> identityWarning["Verification warning\nnếu identity nhập tay"]
        checkSubmission --> semanticLLM["Optional LLMSemanticValidator"]

        required --> issues["Issues"]
        identity --> issues
        dateCheck --> issues
        signature --> issues
        procedureRules --> issues
        identityWarning --> issues
        semanticLLM --> issues

        issues --> dedupe["Dedupe issues"]
        dedupe --> validationResponse["ready_to_submit,\nverification_status,\nvalidation_layers,\nanswer"]
        validationResponse --> ui
    end
```

Các điểm cố ý không vẽ như module riêng:

- Không có stage `Reranker` riêng trong code hiện tại.
- Không có stage `Metadata Filter` runtime riêng trong code hiện tại.
- `AI Orchestrator` trong flow cũ được thay bằng các hàm/thành phần thật: `ChatService`, `ProcedureAssistant`, `ChromaRetriever`, `SubmissionValidator`.
