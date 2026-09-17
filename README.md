# Switchbuilder

Switchbuilder is a mobile-first AI Android app builder. Users describe an app in normal language, receive a structured app definition, preview it on a phone canvas, save it, reopen it, and continue iterating.

## Run

Requires Node.js 18+:

```bash
cp .env.example .env
node server.js
```

Open `http://localhost:8787`. The server serves the mobile web app and provides the project/API endpoints.

## AI configuration

The app works immediately without a key using a deterministic local generator for development. For real AI generation, configure these environment variables (do not put secrets in `app.js`):

- `AI_API_KEY` — provider API key
- `AI_API_URL` — OpenAI-compatible chat completions URL
- `AI_MODEL` — model name, default `gpt-4o-mini`

The server sends the current app schema and the user's change request, then validates the returned JSON before updating the preview.

## Structured app schema

Generated apps use `version`, `app`, `theme`, `navigation`, and `screens`. Screen elements currently support `text`, `button`, `input`, `list`, `card`, `form`, and `spacer`. This schema is intentionally renderer- and build-worker-friendly.

## Persistence and build boundary

Projects persist as JSON files under `data/projects` through `GET/POST/PUT /api/projects`. The client also keeps an emergency local draft if the server is unavailable. `POST /api/build` is a real service boundary, but returns a clear `501` until `BUILD_WORKER_URL` points to an implemented Android build worker. No fake APK is generated.

For production, replace file persistence with authenticated database storage and implement the build-worker adapter without exposing provider or worker secrets to the browser.
