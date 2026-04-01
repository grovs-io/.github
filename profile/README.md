<p align="center">
  <a href="https://grovs.io">
    <img src="https://s3.eu-north-1.amazonaws.com/grovs.io/full-black.svg" alt="Grovs" width="220" />
  </a>
</p>

<p align="center">
  Open-source deep linking, attribution, and analytics for mobile apps.<br/>
  Self-hostable alternative to Branch.io and AppsFlyer.
</p>

<p align="center">
  <a href="https://app.grovs.io">Cloud</a> ·
  <a href="https://docs.grovs.io">Docs</a> ·
  <a href="https://docs.grovs.io/docs/self-hosting">Self-Hosting</a>
</p>

---

Grovs gives you smart links, deferred deep linking, install attribution, analytics, push notifications, and in-app messaging — in one platform you can self-host or use as a managed service.

## Features

- **Smart links** — one link that routes to the right destination on iOS, Android, or web
- **Deferred deep linking** — users land on the right in-app screen, even after installing from the store
- **Attribution** — track installs, reinstalls, and reactivations back to campaigns, referrals, or organic shares
- **Analytics dashboard** — clicks, installs, revenue, and campaign performance in real time
- **Push notifications** — send targeted messages via APNs and FCM from the dashboard
- **In-app messages** — display announcements, promos, or onboarding flows without an app update
- **Revenue tracking** — automatic in-app purchase tracking with Apple and Google webhook integration
- **REST API** — create and manage links programmatically from your server
- **Multi-tenant** — one instance serves multiple apps and teams

## Quick start

### Cloud

Sign up at [app.grovs.io](https://app.grovs.io), create a project, grab your API key, and add the SDK:

```swift
// iOS — add via SPM: https://github.com/grovs-io/grovs-iOS.git
Grovs.configure(APIKey: "your-api-key", useTestEnvironment: false, delegate: self)
```

```kotlin
// Android — add io.grovs:Grovs:1.1.0 to build.gradle
Grovs.configure(this, "your-api-key")
```

```dart
// Flutter — add grovs_flutter_plugin: ^1.0.1 to pubspec.yaml
final grovs = Grovs();
```

```javascript
// Web — npm install grovs
const grovs = new Grovs('your-api-key', (data) => console.log(data));
grovs.start();
```

```bash
# React Native
npm install react-native-grovs-wrapper
# Then follow iOS + Android native setup in the docs
```

Full setup guides: [iOS](https://docs.grovs.io/docs/sdk/ios/quick-start) · [Android](https://docs.grovs.io/docs/sdk/android/quick-start) · [Flutter](https://docs.grovs.io/docs/sdk/flutter/quick-start) · [React Native](https://docs.grovs.io/docs/sdk/react-native/quick-start) · [Web](https://docs.grovs.io/docs/sdk/web/quick-start)

### Self-hosted

```bash
git clone https://github.com/grovs-io/backend.git && cd backend
cp .env.example .env
docker compose run --rm web bin/rails db:encryption:init
# Copy the three generated keys into .env
docker compose up --build
docker compose exec web bundle exec rails db:create db:migrate db:seed
```

This starts the backend on port 8765 with PostgreSQL, Redis, and five Sidekiq workers. Then deploy the [dashboard](https://github.com/grovs-io/dashboard) and point your SDKs to your domain:

```swift
Grovs.configure(APIKey: "key", useTestEnvironment: false, baseURL: "https://yourdomain.com", delegate: self)
```

Full guide: [Backend](https://docs.grovs.io/docs/self-hosting/backend) · [Dashboard](https://docs.grovs.io/docs/self-hosting/dashboard) · [SDK config](https://docs.grovs.io/docs/self-hosting/sdk-configuration)

## Architecture

The backend is a Rails app with subdomain-based routing. A single server handles everything:

- `api.*` — dashboard API (OAuth2)
- `sdk.*` — mobile SDK endpoints
- `go.*` — short link redirects
- `preview.*` — link preview pages

Background processing runs on five dedicated Sidekiq workers for event ingestion, batch processing, scheduling, device updates, and maintenance.

```
┌──────────────┐       ┌─────────────────────────────────┐
│  Dashboard   │─────▶ │        Backend (Rails)          │
│  (Next.js)   │       │                                 │
└──────────────┘       │  api · sdk · go · preview       │
                       │                                 │
┌──────────────┐       │  ┌────────────┐  ┌────────────┐ │
│  Mobile SDKs │─────▶ │  │ PostgreSQL │  │ Redis +    │ │
│  & Web SDK   │       │  │            │  │ Sidekiq    │ │
└──────────────┘       │  └────────────┘  └────────────┘ │
                       └─────────────────────────────────┘
```

## SDKs

| Platform | Install | Source |
|----------|---------|--------|
| iOS | SPM / CocoaPods | [grovs-io/grovs-iOS](https://github.com/grovs-io/grovs-iOS) |
| Android | `implementation("io.grovs:Grovs:1.1.0")` | [grovs-io/grovs-android](https://github.com/grovs-io/grovs-android) |
| Flutter | `grovs_flutter_plugin: ^1.0.1` | [grovs-io/grovs-flutter](https://github.com/grovs-io/grovs-flutter) |
| React Native | `npm install react-native-grovs-wrapper` | [grovs-io/grovs-react-native](https://github.com/grovs-io/grovs-react-native) |
| Web | `npm install grovs` | [grovs-io/grovs-web](https://github.com/grovs-io/grovs-web) |

## Repos

| Repo | What it is |
|------|-----------|
| [backend](https://github.com/grovs-io/backend) | Rails API, link engine, attribution, workers |
| [dashboard](https://github.com/grovs-io/dashboard) | Next.js web dashboard |

## Docs

Full documentation at [docs.grovs.io](https://docs.grovs.io):

- [Getting started](https://docs.grovs.io/docs/getting-started/introduction)
- [Self-hosting guide](https://docs.grovs.io/docs/self-hosting)
- [REST API reference](https://docs.grovs.io/docs/api-reference)
- [Migration from Firebase Dynamic Links](https://docs.grovs.io/docs/migration-guides/firebase-dynamic-links)
- [Migration from Branch.io](https://docs.grovs.io/docs/migration-guides/branch-io)

## License

Core is [MIT](LICENSE). Enterprise features (the `ee/` directory in the backend) require a separate license for production use.
