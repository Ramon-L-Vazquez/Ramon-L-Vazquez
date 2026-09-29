# Howlsy Engineering Evidence

This page is a public evidence index for a private production repository. It summarizes verifiable engineering work without publishing proprietary source, secrets, private datasets, or release configuration.

## What the project demonstrates

Howlsy is not presented here as a single monolithic code drop. Its engineering record is organized around independently developed and validated capabilities:

- authenticated AI-backed APIs for browser and native clients
- native iOS and Android implementations
- simulator and emulator validation
- account deletion and data lifecycle controls
- StoreKit 2 subscription client foundations
- server-owned entitlement authorization foundations
- shared React/TypeScript/CSS mobile presentation inside native store shells
- secure native/web bridges with origin restrictions
- deterministic knowledge-ingestion and provenance safeguards
- CI gates, readiness scripts, and regression coverage

## Evidence by engineering area

### Backend security

The backend was hardened before mobile clients were allowed to call expensive or user-specific AI endpoints. Authentication supports browser sessions and native bearer tokens, while privileged credentials remain server-side.

Evidence represented in repository history:

- protected generation and enrichment endpoints
- explicit unauthorized responses before handler execution
- auth forwarding for streaming generation
- regression coverage for protected routes

### iOS engineering

The iOS work progressed through a real native SwiftUI client before later evolving into the current shared-presentation architecture.

Validated capabilities included:

- Supabase session restoration
- sign-in and account creation
- native callback handling
- authenticated API access
- streamed project generation
- project library/detail flows
- guided progress persistence
- hosted simulator validation

The production architecture later retained Swift for the App Store/OS boundary while moving visible product UI into the shared React/TypeScript/CSS experience.

### Android engineering

The Android client similarly progressed through a Kotlin/Jetpack Compose implementation and hosted emulator validation before the production presentation boundary was simplified.

Validated capabilities included:

- authenticated project generation
- encrypted refresh-token handling
- project library and details
- guided progress updates
- hosted emulator UI testing
- native document-picker integration
- secure origin-scoped WebView bridge behavior

### Mobile architecture evolution

The mobile design deliberately separates presentation ownership from platform ownership.

Shared layer:

- React
- TypeScript
- CSS
- authentication/session UX
- project UX
- API integration

Native iOS/Android layer:

- store packaging
- safe areas
- native haptics
- approved external navigation
- document/file selection
- platform lifecycle behavior
- secure web/native bridge

This avoids maintaining multiple parallel visual implementations while preserving native access to OS capabilities.

### Billing and entitlement security

The billing architecture treats purchase UI and authorization as separate concerns.

Client responsibilities include StoreKit product display, purchasing, restoring, and observing transaction state. Server-side entitlement state is designed as the authorization source for paid features.

Security constraints include:

- entitlement ownership scoped to authenticated users
- RLS
- client write revocation
- server-only privileged queries
- no client-side self-granting of paid access
- explicit handling of active/grace-period states
- separation between client purchase flow and trusted transaction verification

### CI and validation

Howlsy uses multiple validation layers rather than relying on manual clicking:

- TypeScript checks
- production-readiness scripts
- regression gates
- Xcode simulator compilation and UI smoke coverage
- Gradle builds and Android emulator coverage
- native bridge checks
- failure-state tests
- source/provenance regressions
- idempotence checks for knowledge workflows

## Public supporting documents

- [Howlsy Engineering Case Study](./HOWLSY_CASE_STUDY.md)
- [Architecture Overview](./HOWLSY_ARCHITECTURE.md)
- [Engineering Timeline](./HOWLSY_ENGINEERING_TIMELINE.md)
- [Sanitized Engineering Patterns](./HOWLSY_ENGINEERING_PATTERNS.md)

## Why the production repository is private

The private repository contains active product implementation, release configuration, private data workflows, and business logic. This evidence set is intentionally designed to demonstrate engineering scope and reasoning without exposing sensitive implementation details.

---

**Ramon Vazquez**  
Software Development • Full Stack • Mobile • AI Systems • QA / Validation
