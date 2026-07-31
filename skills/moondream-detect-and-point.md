---
name: Detect and locate objects in an image
description: Use Moondream Cloud to detect objects (bounding boxes) and get precise center points for a named object class.
api: openapi/moondream-openapi.yml
operations: [detect, point]
---

# Detect and locate objects (Moondream Detect + Point)

Find objects in an image and locate them, using Moondream's `detect` and `point` skills.

## Prerequisites
- A Moondream API key (https://moondream.ai/c/cloud/api-keys).
- An image as a data URL or public URL, and the object class you want to find (e.g. `"person"`, `"moon"`).

## Steps
1. Detect bounding boxes: `POST https://api.moondream.ai/v1/detect` (operation `detect`).
   - Header: `X-Moondream-Auth: <API_KEY>`
   - Body: `{ "model": "moondream3.1-9B-A2B", "image_url": "<image>", "object": "<class>" }`
   - Response `objects[]` are normalized boxes `{x_min, y_min, x_max, y_max}` in 0..1.
2. Get center points: `POST https://api.moondream.ai/v1/point` (operation `point`) with the same body shape.
   - Response `points[]` are normalized centers `{x, y}` in 0..1.
3. Multiply normalized coordinates by the image width/height to map back to pixels.

## Rules
- Coordinates are normalized (0..1) relative to image dimensions — never assume pixels.
- Required fields: `model`, `image_url`, `object`. Missing fields return HTTP 400.
- Respect rate limits (2/sec base, 10/sec funded); retry with backoff on HTTP 429.
- For a mask instead of a box, use the `segment` skill (returns an SVG `path` + `bbox`).
