# Switchbuilder real APK Autopilot

The flow is now: natural-language prompt → AI/local generator → validated schema → isolated Android project generation → real Gradle build → verified APK artifact → download.

## Build host requirements

The server detects Java, Gradle, and `ANDROID_SDK_ROOT`/`ANDROID_HOME`. A real build requires Node 18+, Java 17+, Gradle 8.7+, Android SDK platform 35/build tools, and network access for the Android Gradle Plugin on first use. Configure `GRADLE_COMMAND` and `BUILD_TIMEOUT_MS` in the environment.

If prerequisites are absent, builds enter `generating` and fail honestly with the detected capability result. No APK or download URL is created.

## API

- `POST /api/generate` — generates and validates schema
- `POST /api/build` — validates schema and returns a queued build id
- `GET /api/builds/:id` — status, progress, actual logs and errors
- `GET /api/builds/:id/download` — available only after a non-empty APK is verified
- `GET /api/capabilities` — build-host capability detection

Generated projects are isolated under `data/builds/<build-id>/project`; artifacts are stored under the build directory and are ignored by Git. The worker uses a fixed `gradle --no-daemon --stacktrace assembleRelease` command and never executes prompt content.
