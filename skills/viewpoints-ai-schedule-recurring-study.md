---
name: Schedule a recurring Viewpoints study
description: Create or update a recurring run schedule for an existing source study, and read its next scheduled run.
api: openapi/viewpoints-ai-study-openapi.json
operations:
  - put__v1_study-schedules_{study_id}
  - get__v1_study-schedules_{schedule_id}
---

# Schedule a recurring Viewpoints study

Turn an existing study into a repeating panel so you can track how a simulated audience responds over time.

## Authentication
Send your API key in the `X-API-Key` header (or a bearer JWT).

## Steps

1. **Create or update the schedule.** Call `put__v1_study-schedules_{study_id}` with the source study's id, setting `enabled` and `repeat_interval`. This upserts the schedule bound to that source study.
2. **Read the schedule.** Call `get__v1_study-schedules_{schedule_id}` to confirm `enabled`, `repeat_interval`, `source_study_id`, and `next_scheduled_run`.

## Error handling
`401` = bad/missing key; `403` = not authorized; `404` = study/schedule not found. Errors use the `{ "error": string, "details": string|null }` envelope (see `errors/viewpoints-ai-problem-types.yml`).
