# Live Demo Runbook

## Public endpoints

- Portal: `https://gov-ease-ai.vercel.app`
- Widget: `https://gov-ease-ai.vercel.app/widget`
- API: `https://govease-ai.onrender.com`
- API readiness: `https://govease-ai.onrender.com/api/v1/ready`

## Three-minute scenario

1. Enter: `Tôi mới sinh con và muốn đăng ký khai sinh.`
2. Show the selected procedure, official source, required documents and ordered steps.
3. Continue to pre-submission checking.
4. Enter an invalid identity number, a birth date that conflicts with the birth certificate, or omit a required confirmation.
5. Run validation and show field-level explanations and suggested fixes.
6. Correct the fields and run validation again until the form is ready.
7. Open `/widget` or embed `public/embed.js` to demonstrate integration without installing an application.

## Pre-demo checks

```powershell
curl.exe -f https://govease-ai.onrender.com/api/v1/health
curl.exe -f https://govease-ai.onrender.com/api/v1/ready
curl.exe -i -X OPTIONS "https://govease-ai.onrender.com/api/v1/intake" `
  -H "Origin: https://gov-ease-ai.vercel.app" `
  -H "Access-Control-Request-Method: POST" `
  -H "Access-Control-Request-Headers: content-type"
```

Health only proves that the API process is reachable. Inspect the readiness JSON and continue with the full RAG demo only when both conditions hold:

- top-level `status` is `ready`;
- `components.index.status` is `ready`, with a non-empty `active_collection` and positive `chunk_count`.

If readiness is `degraded`, the catalog and deterministic fallback may still work, but do not present vector retrieval as production-ready. Wait for background initialization, inspect Render logs, and follow `render-deployment.md` if the index remains in `building` or changes to `failed`.

The CORS preflight must return `200` and the exact Vercel origin. Keep a short screen recording as fallback for cold-start or venue-network failures.

## Honest pilot boundary

The end-to-end field-validation pilot covers birth registration (`1.001193`) and temporary-residence registration (`1.004194`). The other 18 PDF-derived catalog entries provide grounded guidance and must not be presented as having the same validation depth. Guidance remains advisory and links directly to the corresponding National Public Service Portal procedure page plus archived PDF provenance.
