# Howlsy Engineering Case Study

> Public technical case study for a private production repository.

## Overview

Howlsy is an AI-powered project assistant and knowledge platform designed to turn a user's goal and intake details into a structured project, relevant resources, and guided execution. The production repository remains private, so this case study documents the engineering architecture, development milestones, validation strategy, and technical decisions without exposing proprietary source code or credentials.

## Engineering Scope

The project spans:

- Next.js, React, TypeScript, and CSS application architecture
- Supabase authentication, persistence, storage, and row-level security
- OpenAI-backed project generation and knowledge-assisted workflows
- Native iOS engineering with Swift, SwiftUI, StoreKit 2, WKWebView, and secure native/web bridging
- Native Android engineering with Kotlin, Jetpack Compose prototypes, WebView integration, AndroidX WebKit, secure bridge messaging, and native file-picker support
- Protected API routes with web cookie and native bearer-token authentication
- Account deletion and privacy-oriented data lifecycle work
- Subscription entitlement architecture designed around server-owned authorization
- GitHub Actions CI, regression testing, simulator/emulator validation, and production-readiness gates
- Manufacturer/model knowledge ingestion, provenance controls, exact-fitment safeguards, and deterministic publishing workflows

## Architecture Evolution

### 1. Web and backend foundation

Howlsy began as a shared web application backed by Supabase and AI-assisted project generation. The architecture emphasizes authenticated access, structured persisted project data, provenance-aware knowledge retrieval, and testable production boundaries.

### 2. Protected mobile API contract

The backend was hardened so costly and user-specific AI endpoints accept authenticated sessions from both the web application and native clients. Native clients use Supabase bearer tokens, while the web application continues to use its browser session.

### 3. Native iOS shell

A native iOS client was added using SwiftUI and Supabase authentication. The work included session restoration, email/password authentication, native callback handling, authenticated API access, streaming project-generation progress, project browsing, project detail views, and simulator build validation.

### 4. Native Android shell

A Kotlin/Jetpack Compose Android implementation followed, including authenticated project generation, encrypted refresh-token handling, guided-mode progress, project browsing, emulator UI coverage, and CI validation.

### 5. Shared presentation architecture

After native system boundaries were validated, the mobile presentation architecture transitioned toward a shared React/TypeScript/CSS interface hosted inside native iOS and Android store shells. Swift and Kotlin remain responsible for OS-level capabilities and store packaging while the visible product experience is controlled through one shared presentation system.

The native boundary uses origin-restricted messaging rather than unrestricted JavaScript bridges. Android file uploads are routed through the native document picker, external navigation is restricted, and the bridge exposes only narrowly defined OS actions.

## Security and Account Architecture

Key security work includes:

- authenticated protection for AI-backed endpoints
- cookie-session support for web and bearer-token support for native clients
- live user validation before privileged operations
- service-role credentials kept server-side
- account deletion flows that remove owned cloud data
- RLS-protected entitlement records
- client write revocation for billing entitlement state
- user-scoped privileged server queries
- origin-scoped native bridge messaging
- restricted external navigation and explicit native capability allowlists

## App Store and Billing Foundations

The iOS work includes a StoreKit 2 subscription client foundation with:

- monthly and annual subscription product support
- localized App Store product pricing
- purchase, restore, entitlement, and transaction-update flows
- App Store account-token binding to the signed-in Howlsy user
- protection against granting entitlement to the wrong Howlsy account
- local StoreKit configuration for development testing

The backend includes a server-owned billing entitlement foundation with read-only client access and restricted mutation paths. Trusted server-side transaction verification and reconciliation are treated as separate release requirements rather than being simulated in the client.

## Validation Strategy

Howlsy uses layered validation rather than relying on manual testing alone.

### Web / shared application

- TypeScript validation
- regression scripts
- production-readiness checks
- authentication and authorization checks
- deterministic data-path tests

### iOS

- Xcode simulator compilation
- Swift syntax validation
- native shell smoke coverage
- StoreKit readiness regression checks

### Android

- Gradle compilation
- hosted emulator UI validation
- WebView/native bridge verification
- native file-picker path checks

### Data / knowledge system

- exact-model source validation
- provenance checks
- restrictive source-rights handling
- idempotent staging and publication workflows
- neighbor-model fitment protection
- deterministic regression coverage

## Selected Development Milestones

The private repository contains a long-running pull-request-driven development history. Recent major milestones include:

| PR | Milestone | Status | Commits |
|---|---|---:|---:|
| #88 | App Store account deletion and legal readiness | Merged | 3 |
| #89 | Protected AI APIs for web and native sessions | Merged | 5 |
| #90 | Native SwiftUI Howlsy shell | Merged | 49 |
| #91 | Native Android Howlsy shell | Merged | 35 |
| #93 | StoreKit subscription client foundation | Open | 12 |
| #94 | Server-owned billing entitlement foundation | Open | 4 |
| #95 | Shared CSS presentation shell for mobile apps | Open | 71 |

These milestones represent incremental engineering work rather than a single code dump: backend security, platform clients, mobile architecture, billing foundations, testing, and production-readiness work were developed and validated separately.

## Technologies

**Languages**  
TypeScript • JavaScript • Swift • Kotlin • SQL • HTML • CSS

**Frameworks / Platforms**  
Next.js • React • SwiftUI • StoreKit 2 • Android • Jetpack Compose • WKWebView • AndroidX WebKit

**Backend / Data**  
Supabase • PostgreSQL • Row-Level Security • Server-side API routes

**AI / Knowledge**  
OpenAI integration • structured retrieval • provenance controls • deterministic validation

**Engineering / Delivery**  
Git • GitHub • GitHub Actions • simulator/emulator testing • regression gates • release runbooks

## Engineering Principles Demonstrated

- protect expensive and user-specific APIs before exposing native clients
- keep privileged credentials and entitlement mutations server-side
- validate platform-specific behavior independently
- use narrowly scoped native bridges instead of unrestricted interfaces
- preserve provenance and exact-fitment boundaries in knowledge systems
- make retries and publication idempotent
- separate prototype architecture from production architecture when requirements evolve
- use pull requests and CI as part of the engineering record, not only as deployment mechanics

## Repository Visibility

The production Howlsy repository is intentionally private. This public case study is maintained to provide a verifiable overview of the engineering scope while protecting proprietary implementation details, credentials, private datasets, and release configuration.

---

**Built by Ramon Vazquez**  
Software Development • AI Systems • Full Stack Engineering • Mobile Engineering • QA Automation
