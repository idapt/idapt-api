---
name: idapt-api
description: Use the idapt REST API to manage chats, agents, projects, files, images, speech, web search, triggers, code execution, notifications, and more programmatically.
icon: book
version: "1.1"
license: MIT
---

# idapt REST API

REST API for managing chats, agents, projects, files, image generation, speech synthesis, web search, triggers, code execution, notifications, and more programmatically.

## Base URL

```
https://idapt.ai/api/v1
```

## Authentication

Send your API key (starts with `uk_`) in every request:

```
Authorization: Bearer uk_your_key_here
```

Or via header: `x-api-key: uk_your_key_here`

Create API keys in Settings > API Keys. Select only the permissions you need.

## Response Format

All responses use snake_case field names and ISO 8601 timestamps.

```json
// Single resource
{ "data": { "id": "...", "created_at": "2024-01-15T10:30:00.000Z" } }

// List
{ "data": [...], "pagination": { "has_more": true } }

// Delete
{ "deleted": true, "id": "..." }

// Error
{ "error": { "code": "not_found", "message": "Chat not found" } }
```

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
| GET | /projects | List projects |
| POST | /projects | Create a project |
| GET | /projects/:id | Get a project |
| PATCH | /projects/:id | Update a project |
| DELETE | /projects/:id | Delete a project |
| GET | /projects/:id/members | List project members |
| POST | /projects/:id/members | Add a member |
| PATCH | /projects/:id/members/:mid | Update member role |
| DELETE | /projects/:id/members/:mid | Remove a member |
| GET | /files | List files |
| POST | /files | Upload a file (multipart/form-data) |
| GET | /files/:id | Download a file |
| PATCH | /files/:id | Update file metadata |
| DELETE | /files/:id | Trash a file |
| POST | /files/folders | Create a folder (idempotent) |
| POST | /files/:id/move | Move file/folder |
| POST | /files/:id/run | Execute code file in sandbox |
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
| GET | /code-runs | List code runs |
| POST | /code-runs | Execute a code file in sandbox |
| GET | /code-runs/:id | Get code run details |
| GET | /notifications | List your inbox notifications |
| POST | /notifications | Send a notification to project members |
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
curl https://idapt.ai/api/v1/chats \
  -H "Authorization: Bearer uk_your_key_here"
```

### Create an agent

```bash
curl https://idapt.ai/api/v1/agents \
  -H "Authorization: Bearer uk_your_key_here" \
  -H "Content-Type: application/json" \
  -d '{"name": "My Agent", "icon": "bot"}'
```

### Upload a file

```bash
curl https://idapt.ai/api/v1/files \
  -H "Authorization: Bearer uk_your_key_here" \
  -F "file=@report.pdf" \
  -F "project_id=YOUR_PROJECT_ID"
```

### Send a message (sync — waits for AI response)

```bash
curl https://idapt.ai/api/v1/chats/CHAT_ID/messages \
  -H "Authorization: Bearer uk_your_key_here" \
  -H "Content-Type: application/json" \
  -d '{"content": "What is the capital of France?"}'
```

### Send a message (async — returns immediately)

```bash
curl https://idapt.ai/api/v1/chats/CHAT_ID/messages \
  -H "Authorization: Bearer uk_your_key_here" \
  -H "Content-Type: application/json" \
  -d '{"content": "Summarize this project", "wait": false}'
```

### Create a folder

```bash
curl https://idapt.ai/api/v1/files/folders \
  -H "Authorization: Bearer uk_your_key_here" \
  -H "Content-Type: application/json" \
  -d '{"name": "Documents", "project_id": "YOUR_PROJECT_ID"}'
```

### Move a file

```bash
curl -X POST https://idapt.ai/api/v1/files/FILE_ID/move \
  -H "Authorization: Bearer uk_your_key_here" \
  -H "Content-Type: application/json" \
  -d '{"parent_id": "FOLDER_ID"}'
```

### List project members

```bash
curl https://idapt.ai/api/v1/projects/PROJECT_ID/members \
  -H "Authorization: Bearer uk_your_key_here"
```

### Get usage stats

```bash
curl https://idapt.ai/api/v1/me/usage \
  -H "Authorization: Bearer uk_your_key_here"
```

### List models

```bash
curl https://idapt.ai/api/v1/models \
  -H "Authorization: Bearer uk_your_key_here"
```

### Generate an image

```bash
curl https://idapt.ai/api/v1/images/generations \
  -H "Authorization: Bearer uk_your_key_here" \
  -H "Content-Type: application/json" \
  -d '{"prompt": "A sunset over the ocean", "project_id": "PROJECT_ID"}'
```

### Generate speech

```bash
curl https://idapt.ai/api/v1/audio/speech \
  -H "Authorization: Bearer uk_your_key_here" \
  -H "Content-Type: application/json" \
  -d '{"text": "Hello world", "project_id": "PROJECT_ID"}'
```

### Web search

```bash
curl https://idapt.ai/api/v1/web/search \
  -H "Authorization: Bearer uk_your_key_here" \
  -H "Content-Type: application/json" \
  -d '{"query": "latest AI news"}'
```

### Create a cron trigger

```bash
curl "https://idapt.ai/api/v1/triggers?project_id=PROJECT_ID" \
  -H "Authorization: Bearer uk_your_key_here" \
  -H "Content-Type: application/json" \
  -d '{"trigger_type":"cron","name":"Daily Report","cron_expression":"0 9 * * *","action_type":"agent-run","agent_id":"AGENT_ID","prompt_template":"Generate a daily report"}'
```

### Execute code

```bash
curl https://idapt.ai/api/v1/code-runs \
  -H "Authorization: Bearer uk_your_key_here" \
  -H "Content-Type: application/json" \
  -d '{"file_id": "FILE_ID"}'
```

### List code runs

```bash
curl https://idapt.ai/api/v1/code-runs \
  -H "Authorization: Bearer uk_your_key_here"
```

### Send a notification to project members

```bash
curl https://idapt.ai/api/v1/notifications \
  -H "Authorization: Bearer uk_your_key_here" \
  -H "Content-Type: application/json" \
  -d '{
    "project_id": "PROJECT_ID",
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
curl "https://idapt.ai/api/v1/notifications?unread_only=true" \
  -H "Authorization: Bearer uk_your_key_here"
```

## Error Codes

| Code | Status | Description |
|------|--------|-------------|
| unauthorized | 401 | Missing or invalid API key |
| forbidden | 403 | Key lacks required permissions |
| not_found | 404 | Resource not found |
| invalid_request | 422 | Validation error |
| rate_limit | 429 | Too many requests |

## Permissions

API keys have granular permissions: `chat`, `agents`, `project`, `files`, `completions`, `user`, `triggers`, `notifications`. Select only what you need when creating a key.

## LLM Completions

For LLM completions (chat with models), use the OpenAI-compatible or Anthropic-compatible proxy APIs instead:

- OpenAI format: `https://idapt.ai/api/openai/v1/chat/completions`
- Anthropic format: `https://idapt.ai/api/anthropic/v1/messages`

Both accept the same `uk_` API key and support all 200+ models.

## Links

- OpenAPI spec (JSON): https://idapt.ai/api/v1/docs
- Interactive API reference: https://idapt.ai/developers/api-reference
- REST API guide (auth, examples, errors): https://idapt.ai/help/api-reference
- OpenAI-compatible API guide: https://idapt.ai/help/openai-compatible-api
- Anthropic-compatible API guide: https://idapt.ai/help/anthropic-compatible-api
- Developers hub: https://idapt.ai/developers
- Create API key: https://idapt.ai/#settings
