# Switchbuilder Autopilot APK builds

Switchbuilder now includes a real Android build pipeline. A validated schema is converted into a generated native Android project, Gradle is invoked, and the produced `app-release.apk` is copied to a build artifact directory. The server never reports success unless that APK exists.

## Build prerequisites

Real APK builds require a build host with:

- Node.js 18+
- Java 17+
- Android SDK with platform 35 and build-tools installed
- Gradle 8.7+ available as `gradle`, or set `GRADLE_COMMAND` to an installed Gradle executable
- Network access on the first build so Gradle can resolve the Android Gradle Plugin

Example environment:

```bash
cp .env.example .env
export ANDROID_SDK_ROOT=$HOME/Android/Sdk
export PATH="$PATH:$ANDROID_SDK_ROOT/platform-tools"
node server.js
```

The generated project uses Android Gradle Plugin 8.5.2 and compiles a native Java Android activity. If the host lacks these prerequisites, the UI reports the actual Gradle/startup error and no download is offered.

## Autopilot flow

Prompt → AI/local schema generation → server validation → queued → building → Gradle → verified APK artifact → download.

`POST /api/build` accepts only a valid schema. Builds run in isolated per-build directories under `data/builds`, use a fixed command (`gradle --no-daemon --stacktrace assembleRelease`), have a configurable timeout (`BUILD_TIMEOUT_MS`, default 10 minutes), and never execute prompt text as a command. Successful APKs are available from `GET /api/builds/:id/download`.

Build status is available at `GET /api/builds/:id`. The Android build-worker adapter is intentionally local and synchronous-in-a-background-process; it can later be moved to a queue/worker service without changing the frontend API.

## Configuration

- `PORT` — HTTP port
- `AI_API_KEY`, `AI_API_URL`, `AI_MODEL` — optional AI backend
- `GRADLE_COMMAND` — Gradle executable or absolute path
- `BUILD_TIMEOUT_MS` — build timeout
