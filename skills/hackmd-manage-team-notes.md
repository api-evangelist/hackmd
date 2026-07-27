---
name: Manage HackMD team notes
description: Discover the teams a token can access and create, update and list notes inside a team workspace.
api: openapi/hackmd-openapi-original.json
operations: [GetCurrentUser, ListTeams, ListTeamNotes, CreateTeamNote, UpdateTeamNote, DeleteTeamNote]
---

# Manage HackMD team notes

## Auth
`Authorization: Bearer <API_ACCESS_TOKEN>`; base URL `https://api.hackmd.io/v1`.
The token acts as the issuing user, so it can only reach that user's teams.

## Steps
1. **GetCurrentUser** — `GET /me` to confirm the identity and see `teams`.
2. **ListTeams** — `GET /teams` to enumerate teams; note each team's `path`
   (the `teampath` slug used in every team route).
3. **ListTeamNotes** — `GET /teams/{teampath}/notes`.
4. **CreateTeamNote** — `POST /teams/{teampath}/notes` with the same body shape as a
   personal note (Markdown string or JSON with content/title/tags/permissions).
5. **UpdateTeamNote** — `PATCH /teams/{teampath}/notes/{noteId}`.
6. **DeleteTeamNote** — `DELETE /teams/{teampath}/notes/{noteId}` (moves to team trash;
   recover with the trash endpoints).

## Rules
- `403` means the token's user is not a member of `{teampath}`.
- Same 100-calls / 5-min rate limit and `429` back-off applies.
