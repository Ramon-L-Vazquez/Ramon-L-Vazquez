# Howlsy Engineering Timeline

This is a public, sanitized engineering timeline for the private **Howlsy** production repository. It highlights architecture and validation milestones without exposing proprietary code.

## September 2026

### Secure the AI-backed API boundary — PR #89

Before native mobile clients were allowed to call expensive AI-backed routes, Howlsy added an authenticated API contract.

Key work:

- protected generation, streaming generation, enrichment, and product-search routes
- accepted authenticated browser sessions and native bearer-token sessions
- forwarded authenticated identity through streaming generation
- rejected invalid sessions before protected handlers ran
- kept provider and service-role secrets server-side
- added regression coverage for the native authentication contract

**Engineering theme:** establish the trust boundary before adding mobile clients.

---

### Build the native iOS system prototype — PR #90

A native SwiftUI application was built to validate the entire iPhone application path.

Key work:

- iOS 16 SwiftUI application target
- native Supabase session restore and observation
- sign-in and account creation
- authenticated bearer-token API access
- streamed NDJSON project-generation progress
- cancellation and native completion states
- cloud project library and detail screens
- guided project flows and progress persistence
- hosted simulator build and UI smoke coverage

The PR accumulated **49 commits** before merge.

**Engineering theme:** prove the mobile contracts in a real native client, not only in browser mocks.

---

### Build the native Android system prototype — PR #91

Android received an equivalent native validation path using Kotlin and Jetpack Compose.

Key work:

- native authentication and project-generation flows
- project library and project detail UI
- Android Keystore-encrypted refresh token storage
- expiry-aware session refresh
- guided-mode completion writes
- Gradle CI validation
- hosted Android emulator UI tests

The PR accumulated **35 commits** before merge, and its Android, CI, and knowledge-checkpoint workflows passed at the validated head.

**Engineering theme:** prove platform parity and Android-specific security behavior.

---

### Add native account-deletion and authorization hardening — PR #92

Account-deletion and live-user authorization work expanded the mobile security boundary. The work was later carried forward into the larger mobile architecture transition instead of being discarded.

**Engineering theme:** lifecycle/security features belong in architecture, not as launch-day patches.

---

### Add StoreKit subscription client foundation — PR #93

The iOS subscription client introduced StoreKit 2 behavior while keeping purchase UI separate from server authorization.

Key work:

- monthly and annual subscription product handling
- localized App Store product metadata
- purchase, restore, transaction updates, and current-entitlement handling
- account binding with `appAccountToken`
- mismatch protection between App Store transactions and Howlsy accounts
- local StoreKit configuration for development
- StoreKit readiness regression coverage

**Engineering theme:** validate native commerce mechanics without trusting the client as the paid-feature authority.

---

### Add server-owned billing entitlement foundation — PR #94

A separate server authorization layer was designed for paid access.

Key work:

- billing entitlement schema
- RLS-backed user ownership
- revoked client write privileges
- authenticated read-only entitlement endpoint
- live-user validation before entitlement reads
- user-scoped privileged database query
- explicit accepted entitlement states
- regression coverage for ownership and authorization invariants

**Engineering theme:** purchase state and authorization state are related, but not the same thing.

---

### Transition mobile apps to a shared CSS presentation shell — PR #95

After the SwiftUI and Compose implementations proved the system contracts, Howlsy transitioned its production presentation architecture.

The production direction became:

- one React / TypeScript / CSS product UI
- native Swift and Kotlin store shells
- iOS `WKWebView` and Android `WebView`
- a narrow typed native bridge
- one shared authentication/session owner
- native haptics, external URL handling, and file-picker integration
- same-origin bridge restrictions
- first-party session persistence
- production HTTPS enforcement
- debug-only inspection tooling
- branded retry/failure states
- simulator and emulator contract testing

The earlier SwiftUI and Compose clients remain preserved in Git history as engineering prototypes rather than being treated as wasted work.

PR #95 grew to **70+ commits** as the architecture, mobile security boundary, CI fixtures, and readiness checks were refined.

**Engineering theme:** prototype broadly, validate boundaries, then simplify the production architecture.

---

## What this progression demonstrates

The important part of the project is not the number of commits by itself. The development history shows an engineering process:

1. secure expensive backend capabilities
2. establish an authenticated native API contract
3. prove iOS behavior natively
4. prove Android behavior natively
5. add automated simulator/emulator validation
6. add account/security lifecycle work
7. separate client purchasing from server entitlement authorization
8. consolidate presentation architecture after system boundaries are understood
9. enforce architecture with automated readiness gates

That sequence is the reason the current production architecture is smaller than the prototypes that preceded it: the native prototypes were used to discover and verify what truly needed to remain native.

---

[Architecture overview](./HOWLSY_ARCHITECTURE.md) · [Howlsy case study](./HOWLSY_CASE_STUDY.md) · [Profile](./README.md)
