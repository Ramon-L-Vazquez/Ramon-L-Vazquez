# Sanitized Howlsy Engineering Patterns

These examples are intentionally simplified and sanitized. They illustrate architectural patterns used in Howlsy without reproducing proprietary production source, credentials, endpoints, or release configuration.

## 1. Dual-mode authenticated API guard

```ts
export async function requireAuthenticatedUser(request: Request) {
  const bearer = request.headers.get("authorization")?.replace(/^Bearer\s+/i, "")

  const user = bearer
    ? await validateBearerSession(bearer)
    : await validateBrowserSession(request)

  if (!user) {
    return {
      ok: false as const,
      response: new Response("Unauthorized", {
        status: 401,
        headers: { "Cache-Control": "no-store" },
      }),
    }
  }

  return { ok: true as const, user }
}
```

**Engineering intent:** browser and native clients can share one protected backend contract without exposing privileged server credentials to either client.

## 2. Server-owned entitlement check

```ts
export async function getEntitlement(userId: string) {
  const row = await privilegedDatabase
    .from("billing_entitlements")
    .select("status, expires_at")
    .eq("user_id", userId)
    .maybeSingle()

  const granted =
    row && ["active", "grace_period"].includes(row.status)

  return {
    entitled: Boolean(granted),
    status: row?.status ?? "none",
  }
}
```

**Engineering intent:** the client can read entitlement state but cannot grant itself access. Authorization is scoped to the authenticated user and controlled by trusted server-side state.

## 3. Narrow native bridge contract

```ts
export type NativeAction =
  | { type: "haptic"; style: "light" | "medium" }
  | { type: "openExternal"; url: string }

export function sendNativeAction(action: NativeAction) {
  window.HowlsyNative?.postMessage(action)
}
```

Native handlers validate both the message origin and the requested action before invoking OS functionality.

**Engineering intent:** expose only explicit native capabilities rather than a broad unrestricted JavaScript interface.

## 4. iOS origin-restricted bridge concept

```swift
func userContentController(
    _ userContentController: WKUserContentController,
    didReceive message: WKScriptMessage
) {
    guard message.frameInfo.isMainFrame else { return }
    guard message.frameInfo.request.url?.host == configuredHost else { return }

    handleApprovedMessage(message.body)
}
```

**Engineering intent:** reject subframe or unexpected-origin messages before any native action is considered.

## 5. Android document-picker boundary

```kotlin
private val filePicker = registerForActivityResult(
    ActivityResultContracts.OpenDocument()
) { uri ->
    uri?.let { deliverSelectedFileToWebView(it) }
}
```

**Engineering intent:** HTML file-selection flows can remain in the shared UI while the actual file access uses the platform-native picker and permission model.

## 6. Production-readiness assertion pattern

```js
const checks = [
  assertProtectedRoute("/api/projects/generate"),
  assertNoClientEntitlementWrites(),
  assertNativeBridgeUsesExplicitOrigins(),
  assertReleaseWebUrlUsesHttps(),
  assertDebuggingDisabledForRelease(),
]

const failures = checks.filter((result) => !result.ok)
if (failures.length) process.exit(1)
```

**Engineering intent:** encode critical architecture and security expectations as repeatable gates instead of relying entirely on documentation or memory.

## 7. Graceful mobile shell failure state

```ts
function classifyShellFailure(status?: number) {
  if (!status) return "connection"
  if (status >= 500) return "service"
  return "navigation"
}
```

The native shell presents a branded retry state instead of exposing raw WebView/browser errors.

**Engineering intent:** failure handling is part of the product contract and is independently testable.

## 8. Knowledge publication idempotence

```ts
const existing = await findPublishedFact(sourceId, externalKey)

if (existing) {
  return { status: "unchanged", id: existing.id }
}

return publishReviewedFact({
  sourceId,
  externalKey,
  payload,
  provenance,
})
```

**Engineering intent:** retries should not duplicate reviewed facts or corrupt model-specific knowledge relationships.

## What these patterns demonstrate

The recurring theme is explicit boundaries:

- authentication before expensive work
- server-owned authorization state
- minimal native capability exposure
- platform permissions handled by the platform
- security assumptions encoded as tests
- graceful failure states
- deterministic and idempotent data workflows

These examples are portfolio-safe representations of broader production patterns, not verbatim copies of private application source.
