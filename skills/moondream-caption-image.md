---
name: Caption an image
description: Use Moondream Cloud to generate a natural-language caption for an image, with a length hint and optional streaming.
api: openapi/moondream-openapi.yml
operations: [caption]
---

# Caption an image (Moondream Caption)

Generate a natural-language description of an image using Moondream's `caption` skill — useful for alt text, cataloging, and retail descriptions.

## Prerequisites
- A Moondream API key (https://moondream.ai/c/cloud/api-keys).
- An image as a data URL or public URL.

## Steps
1. Send `POST https://api.moondream.ai/v1/caption` (operation `caption`).
   - Header: `X-Moondream-Auth: <API_KEY>`
   - Body: `{ "model": "moondream3.1-9B-A2B", "image_url": "<image>", "length": "normal" }`
2. Read the `caption` string. The response also includes `metrics` (input/output tokens, timings) and `finish_reason`.
3. Set `"length"` to `short`, `normal`, or `long` to control verbosity; set `"stream": true` for token-by-token output.

## Rules
- `model` and `image_url` are required; `length` defaults to `normal`.
- Handle HTTP 401 (bad key) and 429 (rate limit) with a clear message and backoff.
- Use `metrics.output_tokens` to reason about cost — Cloud is billed per token.
