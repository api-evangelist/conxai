---
name: Run firestop detection on a site image
description: Upload a construction image to Conxai's Firestop API and retrieve the AI firestop solution for a selected point.
api: openapi/conxai-firestop-openapi.json
base_url: https://firestop.conxai.ai
auth: X-Api-Key header
operations:
  - POST /images
  - GET /images/{imageId}/solutions
  - PUT /images/{imageId}/solutions
---

# Firestop detection

Detect and classify firestop penetrations in a site photo using Conxai SiteLens.

## Auth
Send `X-Api-Key: <your-key>` on every request.

## Steps
1. **Request an upload URL** — `POST /images`. The endpoint returns a presigned
   S3 URL. Follow up with a `PUT` of the raw image bytes to that URL, resending
   the **same `Content-Type`** header you sent to `POST /images`.
2. **Get the AI solution** — `GET /images/{imageId}/solutions` with the clicked
   point coordinates. The response includes the RLE mask of the selected object
   and the recommended firestop solution.
3. **Validate the solution** — `PUT /images/{imageId}/solutions` to confirm or
   correct the final solution for that image and point, feeding the result back
   to Conxai.

## Conventions & errors
- Two-step presigned-S3 upload flow; see `conventions/conxai-conventions.yml`.
- Errors return `{ "error": "..." }`; see `errors/conxai-problem-types.yml`.
