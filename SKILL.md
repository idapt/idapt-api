---
name: idapt-api
description: Use the idapt REST API to manage chats, agents, workspaces, Drive (cloud files), images, speech, web search, triggers, code execution, notifications, and more programmatically.
icon: book
version: "1.1"
license: MIT
---

# idapt REST API

REST API for managing chats, agents, workspaces, Drive (cloud files), image generation, speech synthesis, web search, triggers, code execution, notifications, and more programmatically.

## Base URL

```
https://idapt.app/api/v1
```

## Authentication

Send your API key (starts with `uk_`) in every request:

```
Authorization: Bearer uk_your_key_here
```

Or via header: `x-api-key: uk_your_key_here`

Create API keys in Settings > API Keys. Select only the permissions you need.

## Response Format

All responses use snake_case field names and ISO 8601 timestamps. Every
response also carries `X-Request-Id` and `X-Idapt-Version` headers.

```json
// Single resource
{ "data": { "id": "...", "created_at": "2024-01-15T10:30:00.000Z" } }

// List — pagination carries has_more + next_cursor (no total count)
{ "data": [...], "pagination": { "has_more": true, "next_cursor": "..." } }

// Delete
{ "deleted": true, "id": "..." }

// Error — type is the coarse category; code an optional finer sub-code
{ "error": { "type": "not_found", "message": "Chat not found" } }
```

Pagination is cursor-based: list endpoints take `limit` (max 100) and an
optional opaque `cursor`. Page by passing the previous response's
`pagination.next_cursor` as the next request's `?cursor=` — never parse
the cursor. `next_cursor` is `null` once `has_more` is `false`.

## Endpoints

| Method | Path | Description |
|--------|------|-------------|
| GET | /me | Get current user info |
| GET | /me/usage | Get usage stats (storage, history) |
| GET | /chats | List chats |
| POST | /chats | Create a chat |
| GET | /chats/:id | Get a chat |
| PATCH | /chats/:id | Update a chat |
| DELETE | /chats/:id | Delete a chat |
| GET | /chats/:id/messages | List messages |
| POST | /chats/:id/messages | Send a message and get AI response |
| GET | /chats/:id/runs | List agent execution runs |
| GET | /chats/:id/cost | Get cost breakdown |
| GET | /agents | List agents |
| POST | /agents | Create an agent |
| GET | /agents/:id | Get an agent |
| PATCH | /agents/:id | Update an agent |
| DELETE | /agents/:id | Delete an agent |
| GET | /workspaces | List workspaces |
| POST | /workspaces | Create a workspace |
| GET | /workspaces/:id | Get a workspace |
| PATCH | /workspaces/:id | Update a workspace |
| DELETE | /workspaces/:id | Delete a workspace |
| GET | /workspaces/:id/members | List workspace members |
| POST | /workspaces/:id/members | Add a member |
| PATCH | /workspaces/:id/members/:mid | Update member role |
| DELETE | /workspaces/:id/members/:mid | Remove a member |
| GET | /drive/files | List files |
| POST | /drive/files | Upload a file (multipart/form-data) |
| GET | /drive/files/:id | Download a file |
| PATCH | /drive/files/:id | Update file metadata |
| DELETE | /drive/files/:id | Trash a file |
| POST | /drive/files/folders | Create a folder (idempotent) |
| POST | /drive/files/:id/move | Move file/folder |
| GET | /models | List available LLM models |
| GET | /search?q=... | Search across resources |
| POST | /audio/transcriptions | Transcribe audio file |
| POST | /audio/speech | Generate speech from text (TTS) |
| POST | /images/generations | Generate an image from a text prompt |
| GET | /images/models | List image generation models |
| POST | /web/search | Search the web (Brave Search) |
| GET | /triggers | List triggers |
| POST | /triggers | Create a trigger (cron/webhook) |
| GET | /triggers/:id | Get a trigger |
| PATCH | /triggers/:id | Update a trigger |
| DELETE | /triggers/:id | Delete a trigger |
| POST | /triggers/:id/fire | Fire a webhook trigger |
| GET | /triggers/:id/runs | List trigger fire history |
| POST | /triggers/:id/rotate-secret | Rotate webhook secret |
| POST | /computers/:id/exec | Execute a command on a paired computer |
| GET | /operations | List long-running operations |
| GET | /operations/:id | Get operation details |
| POST | /operations/:id/cancel | Cancel an operation |
| GET | /notifications | List your inbox notifications |
| POST | /notifications | Send a notification to workspace members |
| GET | /notifications/:id | Get one notification |
| PATCH | /notifications/:id | Mark read / archive / unarchive |
| DELETE | /notifications/:id | Delete a notification |
| POST | /notifications/read-all | Mark all notifications as read |
| GET | /notifications/preferences | Channel preference matrix |
| PATCH | /notifications/preferences | Update channel preferences |
| GET | /notifications/config | Quiet hours / toasts / sound |
| PATCH | /notifications/config | Update quiet-hours + toasts + sound |

## Quick Examples

### List chats

```bash
curl https://idapt.app/api/v1/chats \
  -H "Authorization: Bearer uk_your_key_here"
```

### Create an agent

```bash
curl https://idapt.app/api/v1/agents \
  -H "Authorization: Bearer uk_your_key_here" \
  -H "Content-Type: application/json" \
  -d '{"name": "My Agent", "icon": "bot"}'
```

### Upload a file

```bash
curl https://idapt.app/api/v1/drive/files \
  -H "Authorization: Bearer uk_your_key_here" \
  -F "file=@report.pdf" \
  -F "workspace_id=YOUR_PROJECT_ID"
```

### Send a message (sync — waits for AI response)

```bash
curl https://idapt.app/api/v1/chats/CHAT_ID/messages \
  -H "Authorization: Bearer uk_your_key_here" \
  -H "Content-Type: application/json" \
  -d '{"content": "What is the capital of France?"}'
```

### Send a message (async — returns immediately)

```bash
curl https://idapt.app/api/v1/chats/CHAT_ID/messages \
  -H "Authorization: Bearer uk_your_key_here" \
  -H "Content-Type: application/json" \
  -d '{"content": "Summarize this workspace", "wait": false}'
```

### Create a folder

```bash
curl https://idapt.app/api/v1/drive/files/folders \
  -H "Authorization: Bearer uk_your_key_here" \
  -H "Content-Type: application/json" \
  -d '{"name": "Documents", "workspace_id": "YOUR_PROJECT_ID"}'
```

### Move a file

```bash
curl -X POST https://idapt.app/api/v1/drive/files/FILE_ID/move \
  -H "Authorization: Bearer uk_your_key_here" \
  -H "Content-Type: application/json" \
  -d '{"parent_id": "FOLDER_ID"}'
```

### List workspace members

```bash
curl https://idapt.app/api/v1/workspaces/WORKSPACE_ID/members \
  -H "Authorization: Bearer uk_your_key_here"
```

### Get usage stats

```bash
curl https://idapt.app/api/v1/me/usage \
  -H "Authorization: Bearer uk_your_key_here"
```

### List models

```bash
curl https://idapt.app/api/v1/models \
  -H "Authorization: Bearer uk_your_key_here"
```

### Generate an image

```bash
curl https://idapt.app/api/v1/images/generations \
  -H "Authorization: Bearer uk_your_key_here" \
  -H "Content-Type: application/json" \
  -d '{"prompt": "A sunset over the ocean", "workspace_id": "WORKSPACE_ID"}'
```

### Generate speech

```bash
curl https://idapt.app/api/v1/audio/speech \
  -H "Authorization: Bearer uk_your_key_here" \
  -H "Content-Type: application/json" \
  -d '{"text": "Hello world", "workspace_id": "WORKSPACE_ID"}'
```

### Web search

```bash
curl https://idapt.app/api/v1/web/search \
  -H "Authorization: Bearer uk_your_key_here" \
  -H "Content-Type: application/json" \
  -d '{"query": "latest AI news"}'
```

### Create a cron trigger

```bash
curl "https://idapt.app/api/v1/automations?workspace_id=WORKSPACE_ID" \
  -H "Authorization: Bearer uk_your_key_here" \
  -H "Content-Type: application/json" \
  -d '{"trigger_type":"cron","name":"Daily Report","cron_expression":"0 9 * * *","action_type":"agent-run","agent_id":"AGENT_ID","prompt_template":"Generate a daily report"}'
```

### Execute a computer command

```bash
curl https://idapt.app/api/v1/computers/COMPUTER_ID/exec \
  -H "Authorization: Bearer uk_your_key_here" \
  -H "Content-Type: application/json" \
  -d '{"command": "python script.py"}'
```

### List operations

```bash
curl https://idapt.app/api/v1/operations \
  -H "Authorization: Bearer uk_your_key_here"
```

### Send a notification to workspace members

```bash
curl https://idapt.app/api/v1/notifications \
  -H "Authorization: Bearer uk_your_key_here" \
  -H "Content-Type: application/json" \
  -d '{
    "workspace_id": "WORKSPACE_ID",
    "target": "all_members",
    "title": "Build complete",
    "message": "The staging deploy finished successfully.",
    "channels": ["in_app", "web_push"],
    "urgency": "normal",
    "deep_link": { "kind": "chat", "chat_id": "CHAT_ID" }
  }'
```

### List your inbox

```bash
curl "https://idapt.app/api/v1/notifications?unread_only=true" \
  -H "Authorization: Bearer uk_your_key_here"
```

## Error Codes

The error object is `{ error: { type, message, code? } }`. `type` is the
coarse category clients branch on (one of the values below); `code` is an
optional finer computer-branchable sub-code present only on specific
errors (today only `model_not_available_for_tier`, on a `403` from
`POST /images/generations` and `POST /audio/speech`). `invalid_request`
is always `422` — the API never returns `400` for validation errors.

| `type` | Status | Description |
|------|--------|-------------|
| invalid_request | 422 | Validation error |
| unauthorized | 401 | Missing or invalid API key |
| forbidden | 403 | Key lacks required permissions |
| not_found | 404 | Resource not found |
| conflict | 409 | Resource conflict |
| rate_limit | 429 | Too many requests |
| service_unavailable | 503 | An upstream dependency (computer daemon, inference provider) is unreachable — retryable |
| internal_error | 500 | Server error |

## Permissions

API keys have granular permissions: `chat`, `agents`, `workspace`, `drive`, `completions`, `user`, `triggers`, `notifications`. Select only what you need when creating a key.

## LLM Completions

For LLM completions (chat with models), use the OpenAI-compatible or Anthropic-compatible proxy APIs instead:

- OpenAI format: `https://idapt.app/api/openai/v1/chat/completions`
- Anthropic format: `https://idapt.app/api/anthropic/v1/messages`

Both accept the same `uk_` API key and support all 200+ models.

## Links

- OpenAPI spec (JSON): https://idapt.app/api/v1/docs
- Interactive API reference: https://idapt.app/developers/api-reference
- REST API guide (auth, examples, errors): https://idapt.app/help/api-reference
- OpenAI-compatible API guide: https://idapt.app/help/openai-compatible-api
- Anthropic-compatible API guide: https://idapt.app/help/anthropic-compatible-api
- Developers hub: https://idapt.app/developers
- Create API key: https://idapt.app/#settings
