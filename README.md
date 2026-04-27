<p align="center">
  <h1 align="center">idapt API</h1>
  <p align="center">REST API for the idapt AI workspace</p>
</p>

<p align="center">
  <a href="https://idapt.ai/developers/api-reference"><img src="https://img.shields.io/badge/OpenAPI-3.1-green?style=flat-square" alt="OpenAPI 3.1"></a>
  <a href="https://idapt.ai/pricing"><img src="https://img.shields.io/badge/Tier-Pro%20%2B-blue?style=flat-square" alt="Pro+"></a>
</p>

---

Programmatic access to [idapt](https://idapt.ai) — manage chats, agents, projects, files, image generation, speech synthesis, web search, triggers, code execution, and more via a stable REST API.

## Quick Start

### 1. Create an API key

Go to [idapt Settings](https://idapt.ai/#settings) and create a new API key.

### 2. Make your first request

```bash
curl https://idapt.ai/api/v1/me \
  -H "Authorization: Bearer uk_your_key_here"
```

### 3. Explore the API

Browse the [interactive API reference](https://idapt.ai/developers/api-reference) for all endpoints, schemas, and try-it.

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
| **Projects** | | |
| GET | `/api/v1/projects` | List projects |
| POST | `/api/v1/projects` | Create a project |
| GET | `/api/v1/projects/:id` | Get a project |
| PATCH | `/api/v1/projects/:id` | Update a project |
| DELETE | `/api/v1/projects/:id` | Delete a project |
| GET | `/api/v1/projects/:id/members` | List project members |
| POST | `/api/v1/projects/:id/members` | Add a member |
| PATCH | `/api/v1/projects/:id/members/:mid` | Update member role |
| DELETE | `/api/v1/projects/:id/members/:mid` | Remove a member |
| **Files** | | |
| GET | `/api/v1/files` | List files |
| POST | `/api/v1/files` | Upload a file |
| GET | `/api/v1/files/:id` | Download a file |
| PATCH | `/api/v1/files/:id` | Update file metadata |
| DELETE | `/api/v1/files/:id` | Trash a file |
| POST | `/api/v1/files/folders` | Create a folder (idempotent) |
| POST | `/api/v1/files/:id/move` | Move file/folder |
| POST | `/api/v1/files/:id/run` | Execute code file in sandbox |
| **Images** | | |
| POST | `/api/v1/images/generations` | Generate an image from a text prompt |
| GET | `/api/v1/images/models` | List image generation models |
| **Audio** | | |
| POST | `/api/v1/audio/transcriptions` | Transcribe audio file |
| POST | `/api/v1/audio/speech` | Generate speech from text (TTS) |
| **Web** | | |
| POST | `/api/v1/web/search` | Search the web (Brave Search) |
| **Triggers** | | |
| GET | `/api/v1/triggers` | List triggers |
| POST | `/api/v1/triggers` | Create a trigger (cron/webhook) |
| GET | `/api/v1/triggers/:id` | Get a trigger |
| PATCH | `/api/v1/triggers/:id` | Update a trigger |
| DELETE | `/api/v1/triggers/:id` | Delete a trigger |
| POST | `/api/v1/triggers/:id/fire` | Fire a webhook trigger |
| GET | `/api/v1/triggers/:id/runs` | List trigger fire history |
| POST | `/api/v1/triggers/:id/rotate-secret` | Rotate webhook secret |
| **Code Runs** | | |
| GET | `/api/v1/code-runs` | List code runs |
| POST | `/api/v1/code-runs` | Execute a code file in sandbox |
| GET | `/api/v1/code-runs/:id` | Get code run details |
| **Other** | | |
| GET | `/api/v1/models` | List available LLM models |
| GET | `/api/v1/search?q=...` | Search resources |

---

## Quick Examples

### Send a message (sync)

```bash
curl https://idapt.ai/api/v1/chats/CHAT_ID/messages \
  -H "Authorization: Bearer uk_your_key_here" \
  -H "Content-Type: application/json" \
  -d '{"content": "What is the capital of France?"}'
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

### Upload a file

```bash
curl https://idapt.ai/api/v1/files \
  -H "Authorization: Bearer uk_your_key_here" \
  -F "file=@report.pdf" -F "project_id=PROJECT_ID"
```

---

## Response Format

```json
// Single resource
{ "data": { "id": "...", "created_at": "2024-01-15T10:30:00.000Z" } }

// List
{ "data": [...], "pagination": { "has_more": true } }

// Error
{ "error": { "code": "not_found", "message": "Chat not found" } }
```

All field names use `snake_case`. Timestamps are ISO 8601.

---

## LLM Completions

For chat completions with 200+ models, use the compatible proxy APIs:

- **OpenAI format**: `https://idapt.ai/api/openai/v1/chat/completions`
- **Anthropic format**: `https://idapt.ai/api/anthropic/v1/messages`

Both accept the same `uk_` API key. See the [OpenAI-compatible API guide](https://idapt.ai/help/openai-compatible-api) or [Anthropic-compatible API guide](https://idapt.ai/help/anthropic-compatible-api).

---

## Links

- [OpenAPI Spec (JSON)](https://idapt.ai/api/v1/docs) — Machine-readable specification
- [Interactive API Reference](https://idapt.ai/developers/api-reference) — Try requests, explore schemas
- [REST API Guide](https://idapt.ai/help/api-reference) — Auth, examples, error codes
- [OpenAI-compatible API Guide](https://idapt.ai/help/openai-compatible-api) — Use with OpenAI SDKs
- [Anthropic-compatible API Guide](https://idapt.ai/help/anthropic-compatible-api) — Use with Claude Code
- [Developers](https://idapt.ai/developers) — All developer tools
- [idapt CLI](https://github.com/idapt/idapt-cli) — Command-line tool
- [idapt MCP](https://github.com/idapt/idapt-mcp) — MCP integration
- [Pricing](https://idapt.ai/pricing) — Plans and API access
