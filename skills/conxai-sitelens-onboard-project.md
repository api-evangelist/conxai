---
name: Onboard a SiteLens project with cameras and users
description: Create a Conxai SiteLens project, attach a site camera, and grant a teammate access.
api: openapi/conxai-customer-openapi.json
base_url: https://customer.conxai.ai
auth: X-Api-Key header (server-to-server key issued by Conxai support)
operations:
  - POST /project
  - POST /camera
  - GET /projects/:projectId/cameras
  - POST /users/add-to-project
---

# Onboard a SiteLens project

Use the Conxai Customer API to stand up a construction-site monitoring project.

## Auth
Send `X-Api-Key: <your-key>` on every request. Keys are server-to-server and
issued by Conxai support — never expose them in a browser client.

## Steps
1. **Create the project** — `POST /project` with `name`, an optional
   `external_id` (your own identifier), a base64 `image_cover`, and a `settings`
   object. Capture the returned project uuid.
2. **Attach a camera** — `POST /camera` referencing the project, with your
   camera's `cameraExternalId`. SiteLens will begin analysing its feed.
3. **Confirm cameras** — `GET /projects/:projectId/cameras` to verify the camera
   is registered against the project.
4. **Grant access** — `POST /users/add-to-project` with the teammate's email and
   the project id so they can view SiteLens results in the web app
   (https://lens.conxai.ai/).

## Conventions & errors
- Errors return `{ "error": "...", "status": "..." }` (schema `ErrorResponse`) —
  not RFC 9457. See `errors/conxai-problem-types.yml`.
- No idempotency-key is supported; treat creates as non-idempotent and check for
  an existing `external_id` before retrying. See `conventions/conxai-conventions.yml`.
