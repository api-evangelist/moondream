---
name: Ask questions about an image
description: Use Moondream Cloud to answer a natural-language question about an image (visual question answering), with optional streaming.
api: openapi/moondream-openapi.yml
operations: [query]
---

# Ask questions about an image (Moondream Query)

Answer a natural-language question about an image using Moondream's `query` skill.

## Prerequisites
- A Moondream API key from the Cloud Console (https://moondream.ai/c/cloud/api-keys).
- An image as a data URL (base64) or a publicly reachable URL.

## Steps
1. Send `POST https://api.moondream.ai/v1/query` (operation `query`).
   - Header: `X-Moondream-Auth: <API_KEY>`
   - Header: `Content-Type: application/json`
   - Body: `{ "model": "moondream3.1-9B-A2B", "image_url": "<data-url-or-url>", "question": "<your question>" }`
2. Read the `answer` field from the response. Each response also carries a `request_id` for support/tracing.
3. For long answers, set `"stream": true` and consume server-sent `data: {"text": "..."}` events.

## Rules
- The `model` field is required — use `moondream3.1-9B-A2B`, `moondream3-preview`, or a finetune checkpoint `{base_model}/{finetune_id}@{step}`.
- Auth failures return HTTP 401; rate-limit failures return HTTP 429 (2 req/sec base, 10 req/sec at >= $10 balance). Back off and retry on 429.
- Calls are stateless single-shot inferences — there is no idempotency key; a retry re-runs inference.
- Errors use `{ "error": { "message", "type" } }`.
