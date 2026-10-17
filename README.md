<p align="center">
  <h1 align="center">idapt API</h1>
  <p align="center">REST API for the idapt AI workspace</p>
</p>

<p align="center">
  <a href="https://idapt.app/developers/api-reference"><img src="https://img.shields.io/badge/OpenAPI-3.1-green?style=flat-square" alt="OpenAPI 3.1"></a>
  <a href="https://idapt.app/pricing"><img src="https://img.shields.io/badge/Tier-Pro%20%2B-blue?style=flat-square" alt="Pro+"></a>
</p>

---

Programmatic access to [idapt](https://idapt.app) — manage chats, agents, workspaces, Drive (cloud files), image generation, speech synthesis, web search, triggers, code execution, and more via a stable REST API.

## Quick Start

### 1. Create an API key

Go to [idapt Settings](https://idapt.app/#settings) and create a new API key.

### 2. Make your first request

```bash
curl https://idapt.app/api/v1/me \
  -H "Authorization: Bearer uk_your_key_here"
```

### 3. Explore the API

Browse the [interactive API reference](https://idapt.app/developers/api-reference) for all endpoints, schemas, and try-it.

---

## Authentication

Send your API key in every request:

```
Authorization: Bearer uk_your_key_here
```

Or via `x-api-key: uk_your_key_here` header.

---

## Endpoints

| Method | Path | Description |
|--------|------|-------------|
| **User** | | |
| GET | `/api/v1/me` | Current user info |
| GET | `/api/v1/me/usage` | Usage stats (storage, history) |
| **Chats** | | |
| GET | `/api/v1/chats` | List chats |
| POST | `/api/v1/chats` | Create a chat |
| GET | `/api/v1/chats/:id` | Get a chat |
| PATCH | `/api/v1/chats/:id` | Update a chat |
| DELETE | `/api/v1/chats/:id` | Delete a chat |
| GET | `/api/v1/chats/:id/messages` | List messages |
| POST | `/api/v1/chats/:id/messages` | Send message + get AI response |
| GET | `/api/v1/chats/:id/runs` | List agent execution runs |
| GET | `/api/v1/chats/:id/cost` | Get cost breakdown |
| **Agents** | | |
| GET | `/api/v1/agents` | List agents |
| POST | `/api/v1/agents` | Create an agent |
| GET | `/api/v1/agents/:id` | Get an agent |
| PATCH | `/api/v1/agents/:id` | Update an agent |
| DELETE | `/api/v1/agents/:id` | Delete an agent |
| **Workspaces** | | |
| GET | `/api/v1/workspaces` | List workspaces |
| POST | `/api/v1/workspaces` | Create a workspace |
| GET | `/api/v1/workspaces/:id` | Get a workspace |
| PATCH | `/api/v1/workspaces/:id` | Update a workspace |
| DELETE | `/api/v1/workspaces/:id` | Delete a workspace |
| GET | `/api/v1/workspaces/:id/members` | List workspace members |
| POST | `/api/v1/workspaces/:id/members` | Add a member |
| PATCH | `/api/v1/workspaces/:id/members/:mid` | Update member role |
| DELETE | `/api/v1/workspaces/:id/members/:mid` | Remove a member |
| **Drive** | | |
| GET | `/api/v1/drive/files` | List files in Drive |
| POST | `/api/v1/drive/files` | Upload a file to Drive |
| GET | `/api/v1/drive/files/:id` | Download a file |
| PATCH | `/api/v1/drive/files/:id` | Update file metadata |
| DELETE | `/api/v1/drive/files/:id` | Trash a file |
| POST | `/api/v1/drive/files/folders` | Create a folder (idempotent) |
| POST | `/api/v1/drive/files/:id/move` | Move file/folder |
| **Images** | | |
| POST | `/api/v1/images/generations` | Generate an image from a text prompt |
| GET | `/api/v1/images/models` | List image generation models |
| **Audio** | | |
| POST | `/api/v1/audio/transcriptions` | Transcribe audio file |
| POST | `/api/v1/audio/speech` | Generate speech from text (TTS) |
| **Web** | | |
| POST | `/api/v1/web/search` | Search the web (Brave Search) |
| **Triggers** | | |
| GET | `/api/v1/automations` | List triggers |
| POST | `/api/v1/automations` | Create a trigger (cron/webhook) |
| GET | `/api/v1/automations/:id` | Get a trigger |
| PATCH | `/api/v1/automations/:id` | Update a trigger |
| DELETE | `/api/v1/automations/:id` | Delete a trigger |
| POST | `/api/v1/automations/:id/fire` | Fire a webhook trigger |
| GET | `/api/v1/automations/:id/runs` | List trigger fire history |
| POST | `/api/v1/automations/:id/rotate-secret` | Rotate webhook secret |
| **Computers + Operations** | | |
| POST | `/api/v1/computers/:id/exec` | Execute a command on a paired computer |
| GET | `/api/v1/operations` | List long-running operations |
| GET | `/api/v1/operations/:id` | Get operation details |
| POST | `/api/v1/operations/:id/cancel` | Cancel an operation |
| **Other** | | |
| GET | `/api/v1/models` | List available LLM models |
| GET | `/api/v1/search?q=...` | Search resources |

---

## Quick Examples

### Send a message (sync)

```bash
curl https://idapt.app/api/v1/chats/CHAT_ID/messages \
  -H "Authorization: Bearer uk_your_key_here" \
  -H "Content-Type: application/json" \
  -d '{"content": "What is the capital of France?"}'
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

### Upload a file

```bash
curl https://idapt.app/api/v1/drive/files \
  -H "Authorization: Bearer uk_your_key_here" \
  -F "file=@report.pdf" -F "workspace_id=WORKSPACE_ID"
```

---

## Response Format

```json
// Single resource
{ "data": { "id": "...", "created_at": "2024-01-15T10:30:00.000Z" } }

// List — pagination carries has_more + next_cursor (no total count)
{ "data": [...], "pagination": { "has_more": true, "next_cursor": "..." } }

// Error — type is the coarse category; code an optional finer sub-code
{ "error": { "type": "not_found", "message": "Chat not found" } }
```

All field names use `snake_case`. Timestamps are ISO 8601. Pagination is
cursor-based (`limit` max 100, opaque `cursor`) — page by echoing the
previous response's `pagination.next_cursor` back as `?cursor=`;
`next_cursor` is `null` on the last page. Every response carries
`X-Request-Id` and `X-Idapt-Version` headers.

---

## LLM Completions

For chat completions with 200+ models, use the compatible proxy APIs:

- **OpenAI format**: `https://idapt.app/api/openai/v1/chat/completions`
- **Anthropic format**: `https://idapt.app/api/anthropic/v1/messages`

Both accept the same `uk_` API key. See the [OpenAI-compatible API guide](https://idapt.app/help/openai-compatible-api) or [Anthropic-compatible API guide](https://idapt.app/help/anthropic-compatible-api).

---

## Links

- [OpenAPI Spec (JSON)](https://idapt.app/api/v1/docs) — Computer-readable specification
- [Interactive API Reference](https://idapt.app/developers/api-reference) — Try requests, explore schemas
- [REST API Guide](https://idapt.app/help/api-reference) — Auth, examples, error codes
- [OpenAI-compatible API Guide](https://idapt.app/help/openai-compatible-api) — Use with OpenAI SDKs
- [Anthropic-compatible API Guide](https://idapt.app/help/anthropic-compatible-api) — Use with Claude Code
- [Developers](https://idapt.app/developers) — All developer tools
- [idapt CLI](https://github.com/idapt/idapt-cli) — Command-line tool
- [idapt MCP](https://github.com/idapt/idapt-mcp) — MCP integration
- [Pricing](https://idapt.app/pricing) — Plans and API access
