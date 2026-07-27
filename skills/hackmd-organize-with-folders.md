---
name: Organize HackMD notes with folders
description: Create folders, order them, and file notes into folders in a personal HackMD workspace.
api: openapi/hackmd-openapi-original.json
operations: [ListFolders, CreateFolder, GetFolderOrder, UpdateFolderOrder, UpdateNote]
---

# Organize HackMD notes with folders

## Auth
`Authorization: Bearer <API_ACCESS_TOKEN>`; base URL `https://api.hackmd.io/v1`.

## Steps
1. **ListFolders** — `GET /folders` to see existing folders (id, name, icon, color,
   nesting via `parentFolderId`).
2. **CreateFolder** — `POST /folders` with `name` and optional `icon`, `color`,
   `parentFolderId` (nest under an existing folder).
3. **UpdateNote** — `PATCH /notes/{noteId}` setting `parentFolderId` to file a note
   into the folder.
4. **GetFolderOrder / UpdateFolderOrder** — `GET`/`PUT /folders/folder-order` to read
   and rewrite the folder display ordering.

## Rules
- Team folders use the parallel `/teams/{teampath}/folders...` routes.
- `422` on create/update usually means an invalid `parentFolderId` or color/icon value.
- Deleting a folder (`DELETE /folders/{folderId}`) does not delete its notes.
