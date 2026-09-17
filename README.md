# Switchbuilder

Switchbuilder is a mobile-first foundation for a free AI-powered Android app builder. Describe an app in normal language, see a generated phone preview, and continue chatting to iterate.

## Current foundation

- Responsive mobile-first builder workspace
- Build chat with starter prompts and natural-language input
- Generated phone-sized preview with starter UI
- Project naming and local-device save/restore using `localStorage`
- JSON project export
- Android build/export entry point ready for a real generation backend

## Run locally

Open `index.html` in a browser, or serve the folder with any static web server:

```bash
python3 -m http.server 8080
```

Then visit `http://localhost:8080`.

## Next implementation steps

The UI is intentionally a working client foundation rather than a mockup. To produce real Android projects, connect `handlePrompt` in `app.js` to an AI/backend service that returns a structured app schema, render that schema in the preview, persist projects server-side, and add a build worker that packages the schema into an Android project/APK.
