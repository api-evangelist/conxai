---
name: Extract data from a document with DocNostic
description: Upload a construction document to Conxai DocNostic, run extraction, poll status, and download the result.
api: openapi/conxai-docnostic-openapi.json
base_url: https://docs.ui.conxai.ai
auth: X-Api-Key header (or JWT Bearer in the Authorization header)
operations:
  - GET /use-cases
  - POST /projects/{projectId}/use-cases/{useCaseId}/documents
  - POST /projects/{projectId}/use-cases/{useCaseId}/documents/{documentId}/process
  - GET /projects/{projectId}/use-cases/{useCaseId}/documents/{documentId}/status
  - GET /projects/{projectId}/use-cases/{useCaseId}/documents/{documentId}/download
---

# Extract a document with DocNostic

Turn an unstructured construction document (PDF, PNG, JPG) into structured data.

## Auth
Send `X-Api-Key: <your-key>`, or a JWT in `Authorization: Bearer <token>`.

## Steps
1. **Pick a use case** — `GET /use-cases` to list the available extraction use
   cases, then work within a project/use-case you own.
2. **Upload the document** — `POST /projects/{projectId}/use-cases/{useCaseId}/documents`
   returns a presigned S3 URL. `PUT` the raw file bytes to it, resending the
   `Content-Type` and a `Content-Disposition` header with the original filename.
   PNG/JPG are treated as one-page documents; PDFs are split into pages.
3. **Process** — `POST .../documents/{documentId}/process` to run extraction on
   demand (optionally changing the document type).
4. **Poll status** — `GET .../documents/{documentId}/status` until loading,
   splitting and prediction are complete.
5. **Download** — `GET .../documents/{documentId}/download` for a presigned link
   to the original or a markups/bounding-box PDF.

## Conventions & errors
- Paginate list endpoints with `offset` and `limit`.
- Errors return `{ "error": "...", "status": "..." }`; see
  `errors/conxai-problem-types.yml`. Async processing may surface 202/502/504.
