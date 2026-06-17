<a href="https://grovs.io">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="https://s3.eu-north-1.amazonaws.com/grovs.io/full-white.svg">
    <img src="https://s3.eu-north-1.amazonaws.com/grovs.io/full-black.svg" width="120" alt="Grovs">
  </picture>
</a>

### The open-source mobile growth platform.
Deep links, attribution, and campaign analytics. One SDK. Self-host or cloud.

<a href="https://app.grovs.io">Cloud</a> ·
<a href="https://docs.grovs.io">Docs</a> ·
<a href="https://docs.grovs.io/docs/self-hosting">Self-Host</a> ·
<a href="https://grovs.io/pricing">Pricing</a>

---

Branch charges $10,000+/year for deep linking and attribution -- and [shares your users' data with ad networks](https://www.grovs.io/blog/your-mmp-is-selling-your-users-data-i-read-the-privacy-policies-so-you-dont-have-to). Grovs does the same job for $180/year, keeps your data in the EU, and the code is open source.

**Your data stays yours.** No ad network partnerships. No cross-platform tracking. No probabilistic fingerprinting. We make money from subscriptions, not from your users' data. [Read why that matters.](https://www.grovs.io/blog/your-mmp-is-selling-your-users-data-i-read-the-privacy-policies-so-you-dont-have-to)

## Features

- **Smart links** -- one link that routes to the right destination on iOS, Android, or web
- **Deferred deep linking** -- users land on the right in-app screen, even after installing from the store
- **Attribution** -- track installs, reinstalls, and reactivations back to campaigns, referrals, or organic sources
- **Campaigns** -- create, manage, and compare campaign performance
- **Analytics dashboard** -- clicks, installs, revenue, and campaign performance in real time
- **Revenue tracking** -- automatic IAP tracking with Apple and Google webhook integration
- **In-app messages** -- targeted announcements, promos, and onboarding flows without an app update
- **REST API** -- create and manage links programmatically
- **Multi-tenant** -- one instance serves multiple apps and teams

## Roadmap

We're building toward a full mobile growth platform -- everything CleverTap and Branch do, in one open-source product. What's coming:

- [ ] **Event analytics + user journeys** -- custom events, funnels, user flows, session tracking (Q2 2026)
- [ ] **Push campaigns** -- segmentation, scheduling, A/B testing (Q4 2026)
- [ ] **Marketing automation** -- triggered campaigns based on user behavior (2027)

Want to shape what we build next? [Open an issue](https://github.com/grovs-io/backend/issues) or reach out.

## Quick start

### Cloud (2 minutes)

Sign up at [app.grovs.io](https://app.grovs.io), create a project, add the SDK:

```swift
// iOS — SPM: https://github.com/grovs-io/grovs-iOS.git
Grovs.configure(APIKey: "your-api-key", useTestEnvironment: false, delegate: self)
```

```kotlin
// Android — io.grovs:Grovs:1.1.0
Grovs.configure(this, "your-api-key")
```

```dart
// Flutter — grovs_flutter_plugin: ^1.0.1
final grovs = Grovs();
```

```javascript
// Web — npm install grovs
const grovs = new Grovs('your-api-key', (data) => console.log(data));
grovs.start();
```

```bash
# React Native — npm install react-native-grovs-wrapper
```

Setup guides: [iOS](https://docs.grovs.io/docs/sdk/ios/quick-start) · [Android](https://docs.grovs.io/docs/sdk/android/quick-start) · [Flutter](https://docs.grovs.io/docs/sdk/flutter/quick-start) · [React Native](https://docs.grovs.io/docs/sdk/react-native/quick-start) · [Web](https://docs.grovs.io/docs/sdk/web/quick-start)

<br>

### Self-hosted
  
Grovs is fully self-hostable — run the entire platform on your own infrastructure with no feature gates and no data leaving your servers. A single Docker Compose stack brings up everything: the Rails backend, the Next.js dashboard, PostgreSQL, Redis, MinIO object storage, the Sidekiq workers, and a Caddy reverse proxy with automatic TLS. The backend and dashboard come in as git submodules, and all self-hosted behavior is off by default in those repos — it only activates under this stack.

A ~4 vCPU / 8 GB host comfortably handles **150k–200k monthly users**; past that you split the services across machines (managed Postgres, dedicated Redis, external S3) using the same images.

```bash
git clone --recursive https://github.com/grovs-io/self-host.git grovs-self-hosted
cd grovs-self-hosted
./scripts/setup.sh          # generates secrets + prints your admin password
# set your two domains in .env, then:
docker compose --profile standalone build
docker compose --profile standalone up -d
```

▎ You need two separate registrable domains (production + test links), each with a wildcard DNS record. The full guide walks through DNS, env vars, SDK config, and scaling.

Get started with the [self-host repo](https://github.com/grovs-io/self-host) and the [self-hosting guide](https://docs.grovs.io/docs/self-hosting).

<br><br>

## How it compares

| | Branch | AppsFlyer | Grovs |
|---|---|---|---|
| Deep linking | Yes | No | Yes |
| Attribution | Yes | Yes | Yes |
| Campaign analytics | Limited | Yes | Yes |
| Revenue tracking | No | Yes | Yes |
| Open source | No | No | Yes (MIT) |
| Self-hostable | No | No | Yes |
| Data shared with ad networks | [Yes](https://branch.io/policies/privacy-policy/) | [Yes](https://www.appsflyer.com/privacy-policy/) | No |
| Pricing | "Contact sales" | $15K+/yr | From $2k/yr |

## Architecture

Rails backend with subdomain-based routing:

- `api.*` — dashboard API (OAuth2)
- `sdk.*` — mobile SDK endpoints
- `go.*` — short link redirects
- `preview.*` — link preview pages

Background processing via five Sidekiq workers for event ingestion, batch processing, scheduling, device updates, and maintenance.

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
| [mcp-server](https://github.com/grovs-io/mcp) | MCP server for AI-assisted link management |

## Migrating?

- [From Firebase Dynamic Links](https://docs.grovs.io/docs/migration-guides/firebase-dynamic-links)
- [From Branch.io](https://docs.grovs.io/docs/migration-guides/branch-io)

## Docs

[docs.grovs.io](https://docs.grovs.io) — [Getting started](https://docs.grovs.io/docs/getting-started/introduction) · [REST API](https://docs.grovs.io/docs/api-reference) · [Self-hosting](https://docs.grovs.io/docs/self-hosting)

## License

Core is [MIT](LICENSE). Enterprise features (`ee/` directory) require a separate license for production use.
