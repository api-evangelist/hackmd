---
name: Create and publish a HackMD note
description: Create a Markdown note in HackMD, set its read/write and publish permissions, then read it back.
api: openapi/hackmd-openapi-original.json
operations: [CreateNote, GetNote, UpdateNote]
---

# Create and publish a HackMD note

## Auth
Send a personal API access token on every request:
`Authorization: Bearer <API_ACCESS_TOKEN>` (issue one under Settings > API).
Base URL: `https://api.hackmd.io/v1`.

## Steps
1. **CreateNote** — `POST /notes`. Body is either a raw Markdown string or a JSON
   object with `content`, `title`, `tags`, `readPermission`/`writePermission`
   (`owner` | `signed_in` | `guest`), and `commentPermission`. The response returns
   the new note's `id`/permalink.
2. **GetNote** — `GET /notes/{noteId}` to read the stored note. Supports ETag
   conditional requests; cache the ETag and send it to avoid re-transfer.
3. **UpdateNote** — `PATCH /notes/{noteId}` to change `content`, `title`, `tags`, or
   permissions. Writes are last-write-wins (no idempotency key).

## Rules
- Rate limit: 100 calls / 5 minutes (2000/mo Free, 20000/mo Prime); back off on `429`.
- Errors are plain HTTP status codes (`401` bad token, `403` no permission,
  `404` missing note, `422` invalid permission enum). See errors/hackmd-problem-types.yml.
- No pagination on list endpoints; `GET /notes` returns the full array.
