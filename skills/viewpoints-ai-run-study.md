---
name: Run a Viewpoints study and retrieve results
description: Create an AI-simulated research/jury study, poll until it completes, and fetch the participant results.
api: openapi/viewpoints-ai-study-openapi.json
operations:
  - post__v1_studies_uploads_presigned-url
  - post__v1_studies
  - get__v1_studies_jobs_{job_id}
  - get__v1_studies_{id}
---

# Run a Viewpoints study and retrieve results

Use the Viewpoints Study API to run an AI panel (synthetic personas / simulated jurors) against your materials.

## Authentication
Send your API key in the `X-API-Key` header on every request (obtain it from your Viewpoints dashboard account settings). A bearer JWT (`Authorization: Bearer …`) is also accepted.

## Steps

1. **(Optional) Upload non-text stimuli.** Text-only stimuli need no upload. For images, video, audio, or PDFs, call `post__v1_studies_uploads_presigned-url` to get a presigned S3 POST, then upload the file to the returned URL. Handle `429` (upload rate limit) by backing off and retrying.
2. **Create the study.** Call `post__v1_studies` with your `title`, `research_goal`, `segments`, `assigned_materials` (referencing any uploaded media), `question_groups`, and `number_participants`. It returns a creation job. Expect `402` if the request exceeds your plan's entitlement (participants/questions/question type) — reduce the study or upgrade.
3. **Poll for completion.** Call `get__v1_studies_jobs_{job_id}` with the returned job id. It returns `202` while processing (inspect `phase`/`progress`) and `200` when the job has completed or failed (check `status` and `error_message`); on success it carries the `study_id`.
4. **Retrieve results.** Call `get__v1_studies_{id}` with the completed `study_id` to get `study_details` and per-participant `results`.

## Error handling
All errors use the `{ "error": string, "details": string|null }` envelope. `401` = bad/missing key; `403` = not authorized for the resource; `404` = study/job not found. See `errors/viewpoints-ai-problem-types.yml`.
