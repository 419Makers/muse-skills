---
name: "drive_asset_pull"
description: "List and download files from the user's Google Drive folders into the workspace (e.g. pull raw video clips from a named folder). STATUS: SCAFFOLDED — NOT FUNCTIONAL. Google Drive OAuth is pending; do not attempt Drive calls until a connector is connected."
---

# Drive Asset Pull

## Purpose
Pull raw assets (video clips, photos) from named Google Drive folders into the workspace, so content pipelines start from Drive drops instead of manual attachments.

## Status
**SCAFFOLDED — NOT FUNCTIONAL.** There is no Google Drive connector in this environment (verified: no `custom.google-drive`-style connector exists). Do not attempt any Drive API calls until the Auth section below is satisfied.

## Auth
Google Drive access is not connected. Connecting it is a user-approved step:
1. The user must approve a Google Drive connection via `credentials.request_api_access` (OAuth). This cannot be done unilaterally — it needs the user's explicit go-ahead and their Google sign-in.
2. Once connected, scaffold the tooling with `/opt/hatch/skills/skill-creator/bin/scaffold-connector-skill --provider <slug>` and write the `bin/` helpers against the Drive API v3 endpoints below. Do not hand-roll auth.
3. Until then, every Drive operation in the Workflow section is aspirational — describe it, don't execute it.

## Intended Tooling (post-connection)
Google Drive API v3 (`https://www.googleapis.com/drive/v3`), via a `bin/` helper to be written after Auth completes:
- `files.list` with `q="name='<FOLDER>' and mimeType='application/vnd.google-apps.folder'"` → resolve the folder id
- `files.list` with `q="'<FOLDER_ID>' in parents"` → list contents (id, name, mimeType, modifiedTime)
- `files.get` with `alt=media` → download file bytes into the workspace

Required OAuth scope: `drive.readonly` (list + download only — this skill never writes to Drive).

## Workflow (intended usage, once connected)
1. Given a folder name (e.g. "Hat stuff"), resolve its folder id.
2. List its contents; filter to media files (video/image mime types).
3. Download files not already present in the workspace target directory (match by name + modifiedTime to avoid re-downloads).
4. Report what was pulled (file names, sizes, workspace paths) for the content pipeline to consume.

## Operating Rules
1. Do not attempt any Drive call until a working connector exists. If asked to pull assets before then, report the blocker and the exact user step needed (approve the Drive connection).
2. Read-only: this skill never uploads, moves, renames, or deletes Drive files.
3. Never print, log, or persist OAuth tokens or raw credentials.
4. Download targets stay inside the workspace; never write outside it.
