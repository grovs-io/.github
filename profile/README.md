<a href="https://grovs.io">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="https://s3.eu-north-1.amazonaws.com/grovs.io/full-black.svg">
    <img src="https://s3.eu-north-1.amazonaws.com/grovs.io/full-black.svg" width="120" alt="Grovs">
  </picture>
</a>

### Open-source deep linking, attribution, analytics and engagement for mobile apps.

One SDK for iOS, Android, Flutter, React Native, Unity and web. Run it on your own servers or use the cloud.

<a href="https://app.grovs.io">Cloud</a> ·
<a href="https://docs.grovs.io">Docs</a> ·
<a href="https://github.com/grovs-io/self-host">Self-host</a> ·
<a href="https://grovs.io/pricing">Pricing</a>

---

Grovs does what an MMP does, deep links, install attribution and campaign analytics, without sharing your users' data with ad networks. There are no ad-network partnerships, no cross-app tracking and no probabilistic fingerprinting. The cloud service is paid by subscription and hosted in the EU; the self-hosted edition keeps everything on your infrastructure. [Why that matters.](https://www.grovs.io/blog/your-mmp-is-selling-your-users-data-i-read-the-privacy-policies-so-you-dont-have-to)

## Features

- **Smart links.** One link that opens the right destination on iOS, Android or web, with per-platform redirect rules and social previews.
- **Deferred deep linking.** Users land on the right in-app screen after installing from the store.
- **Attribution.** Installs, reinstalls and reactivations tied back to links, campaigns, referrals or organic.
- **Campaigns.** Group links, compare performance, UTM tracking.
- **Analytics.** Built-in and custom dashboards, an event explorer with filters and breakdowns, sessions and retention, served from ClickHouse.
- **Smart banners.** App download banners for your website that open the app on the same content.
- **Custom domains.** Serve links from a domain you or your customers own.
- **Migration from Branch and AppsFlyer.** Old short links resolve upstream on first click, then become native Grovs links.
- **REST API and MCP server.** Create and manage links and read analytics programmatically or from an AI assistant.
- **Multi-tenant.** One instance serves several apps and teams.

**Enterprise Edition** adds engagement and compliance: audience segments, push campaigns, in-app messages and surveys, multi-step journeys across push and in-app, event streaming to your own storage, in-app purchase revenue tracking, single sign-on (OIDC) with SCIM provisioning, and a tamper-evident audit log with a SIEM export API. See [Editions](#editions).

## Quick start

### Cloud

Sign up at [app.grovs.io](https://app.grovs.io), create a project and add the SDK:

```swift
// iOS — SPM: https://github.com/grovs-io/grovs-iOS.git (from: "3.1.0"), or pod 'Grovs', '~> 3.1'
Grovs.configure(APIKey: "your-api-key", useTestEnvironment: false, delegate: self)
```

```kotlin
// Android — implementation("io.grovs:Grovs:3.1.0")
Grovs.configure(this, "your-api-key", useTestEnvironment = false)
```

```yaml
# Flutter — pubspec.yaml. No Dart call: set GrovsApiKey in Info.plist and grovs_api_key in AndroidManifest.xml
dependencies:
  grovs_flutter_plugin: ^3.1.1
```

```bash
# React Native — configured natively, or with the Expo config plugin (apiKey in app.json)
npm install react-native-grovs-wrapper
```

```csharp
// Unity — Package Manager git URL https://github.com/grovs-io/grovs-unity.git#upm/1.0.0, or openupm add com.grovs.sdk
// No init call: set the API key, URI scheme and link domains in Project Settings > Grovs
using GrovsSDK;
Grovs.DeeplinkReceived += link => Debug.Log(link);
```

```javascript
// Web — npm install grovs
import Grovs from "grovs";
await Grovs.configure({ apiKey: "your-api-key" });
```

Setup guides: [iOS](https://docs.grovs.io/docs/sdk/ios/quick-start) · [Android](https://docs.grovs.io/docs/sdk/android/quick-start) · [Flutter](https://docs.grovs.io/docs/sdk/flutter/quick-start) · [React Native](https://docs.grovs.io/docs/sdk/react-native/quick-start) · [Unity](https://docs.grovs.io/docs/sdk/unity/quick-start) · [Web](https://docs.grovs.io/docs/sdk/web/quick-start)

### Self-hosted

The self-hosted Community Edition runs from two published images, `ghcr.io/grovs-io/backend` and `ghcr.io/grovs-io/dashboard`, released together under one version (currently **3.0.0**). One command downloads the stack, asks for your domain and admin email, generates every secret and starts everything: the backend, the dashboard, PostgreSQL, Redis, ClickHouse, the background workers and a Caddy proxy with automatic TLS.

```bash
curl -fsSL https://raw.githubusercontent.com/grovs-io/self-host/main/install.sh | bash
```

Press Enter at the domain prompt to run a local trial on your machine at `http://dashboard.lvh.me:3002`, no DNS or TLS needed. For a real install, point your domain at the host first: the fixed hosts (`dashboard`, `api`, `sdk`, `go`, `mcp`, `preview`) plus a wildcard for project links and one for test links. A separate branded domain for links is supported.

To upgrade, back up first, set `GROVS_VERSION` in `.env` and follow the [upgrade steps](https://github.com/grovs-io/self-host#upgrades); migrations run as part of them. About 4 vCPU and 8 GB of RAM handle roughly 150,000 to 200,000 monthly users on one host; beyond that the same images run against managed PostgreSQL, ClickHouse and Redis.

Everything is in the [self-host repo](https://github.com/grovs-io/self-host): DNS, environment reference, optional features, SDK configuration, backups, scaling, and one-click templates for Railway, Render, Coolify, Dokploy, Easypanel, CapRover and the major clouds.

## Editions

| | Community Edition | Enterprise Edition |
|---|---|---|
| Deep links, attribution, campaigns, analytics and dashboards, smart banners, custom domains, migration, REST API and MCP | Yes | Yes |
| Audience segments | No | Yes |
| Push campaigns, in-app messages and surveys | No | Yes |
| Journeys across push and in-app | No | Yes |
| Event streaming and data export | No | Yes |
| In-app purchase revenue tracking | No | Yes |
| Single sign-on (OIDC) and SCIM provisioning | No | Yes |
| Audit log with SIEM export | No | Yes |
| Source | `backend` and `dashboard` repos, MIT | `ee/` directories, [Enterprise License](https://github.com/grovs-io/backend/blob/main/ee/LICENSE) |
| Image | `ghcr.io/grovs-io/backend` and `dashboard`, public | built with `GROVS_EE=true`, provided under contract |

The Community Edition images are built without the `ee/` code, so enterprise features are not present in them and cannot be switched on by configuration. The cloud service includes the enterprise features on the corresponding plans.

## How it compares

| | Branch | AppsFlyer | Grovs |
|---|---|---|---|
| Deep linking | Yes | No | Yes |
| Attribution | Yes | Yes | Yes |
| Campaign analytics | Limited | Yes | Yes |
| Revenue tracking | No | Yes | Yes, Enterprise |
| Open source | No | No | Yes, MIT core |
| Self-hostable | No | No | Yes |
| Data shared with ad networks | [Yes](https://branch.io/policies/privacy-policy/) | [Yes](https://www.appsflyer.com/privacy-policy/) | No |
| Pricing | Contact sales | Contact sales | [Published](https://grovs.io/pricing) |

## Architecture

```
┌──────────────┐      ┌──────────────────────────────────────────────┐
│  Dashboard   │────▶ │              Backend (Rails)                 │
│  (Next.js)   │      │   api.* · sdk.* · go.* · preview.* · links   │
└──────────────┘      │                                              │
                      │  ┌────────────┐ ┌────────────┐ ┌──────────┐ │
┌──────────────┐      │  │ PostgreSQL │ │ ClickHouse │ │ Redis +  │ │
│  SDKs & web  │────▶ │  │ links,     │ │ events,    │ │ Sidekiq  │ │
└──────────────┘      │  │ users      │ │ analytics  │ │ workers  │ │
                      │  └────────────┘ └────────────┘ └──────────┘ │
                      └──────────────────────────────────────────────┘
```

The backend routes by subdomain: `api.*` serves the dashboard API (OAuth2), `sdk.*` the mobile and web SDKs, `go.*` and the project subdomains resolve links, `preview.*` renders link previews. PostgreSQL holds projects, links, users and devices; ClickHouse is the event store and serves every analytics read; Redis queues events and backs the Sidekiq workers that ingest events, build sessions, rebuild analytics rollups and run maintenance (and, in the Enterprise Edition, deliver push campaigns, journeys and event streams). The dashboard is a Next.js app configured at start, so one image serves every deployment.

## SDKs

| Platform | Install | Source |
|----------|---------|--------|
| iOS | SPM `from: "3.1.0"` / CocoaPods `pod 'Grovs', '~> 3.1'` | [grovs-io/grovs-iOS](https://github.com/grovs-io/grovs-iOS) |
| Android | `implementation("io.grovs:Grovs:3.1.0")` | [grovs-io/grovs-android](https://github.com/grovs-io/grovs-android) |
| Flutter | `grovs_flutter_plugin: ^3.1.1` | [grovs-io/grovs-flutter](https://github.com/grovs-io/grovs-flutter) |
| React Native | `npm install react-native-grovs-wrapper` (3.1.0) | [grovs-io/grovs-react-native](https://github.com/grovs-io/grovs-react-native) |
| Unity | Package Manager `https://github.com/grovs-io/grovs-unity.git#upm/1.0.0` / OpenUPM `com.grovs.sdk` | [grovs-io/grovs-unity](https://github.com/grovs-io/grovs-unity) |
| Web | `npm install grovs` (2.1.0) | [grovs-io/grovs-js](https://github.com/grovs-io/grovs-js) |

## Repos

| Repo | What it is |
|------|-----------|
| [self-host](https://github.com/grovs-io/self-host) | Installer, Compose stack, platform templates and self-hosting guide |
| [backend](https://github.com/grovs-io/backend) | Rails API, link engine, attribution, analytics, workers |
| [dashboard](https://github.com/grovs-io/dashboard) | Next.js web dashboard |
| [mcp](https://github.com/grovs-io/mcp) | MCP server for AI assistants |

Releases are versioned together: a backend, dashboard and self-host tag with the same number belong to one release. Each repo's `CHANGELOG.md` and GitHub release notes list what changed and how to upgrade.

## Roadmap

- [x] Event analytics, sessions and retention
- [x] Self-hosted Community Edition with published images and a one-command installer
- [x] Custom dashboards and smart banners
- [x] Segments, push campaigns, in-app messages and journeys (Enterprise)
- [x] Unity SDK

To influence what comes next, [open an issue](https://github.com/grovs-io/backend/issues).

## Migrating?

- [From Firebase Dynamic Links](https://docs.grovs.io/docs/migration-guides/firebase-dynamic-links)
- [From Branch.io](https://docs.grovs.io/docs/migration-guides/branch)
- [From AppsFlyer](https://docs.grovs.io/docs/migration-guides/appsflyer)

## Docs

[docs.grovs.io](https://docs.grovs.io) — [Getting started](https://docs.grovs.io/docs/getting-started/introduction) · [REST API](https://docs.grovs.io/docs/api-reference) · [Self-hosting](https://docs.grovs.io/docs/self-hosting)

## License

The backend and the dashboard are MIT. The `ee/` directory in each is covered by the [Grovs Enterprise License](https://github.com/grovs-io/backend/blob/main/ee/LICENSE) and requires a commercial agreement for production use.
