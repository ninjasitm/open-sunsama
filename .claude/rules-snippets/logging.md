# Monorepo Logging Standards

## Format

Always include app or package context:

```ts
// App logs
console.log('[App:api] [Source]: Message | details');

// Package logs
console.error('[Pkg:utils] [Service]: Error | context');

// Bad - no context
console.log('message');
```

## Log Levels

- **error**: Critical failures (database, API, cross-app calls)
- **warn**: Non-critical issues (deprecations, version mismatches)
- **info**: Significant events (startup, cross-app communication)
- **debug**: Diagnostic details (cross-package calls, state)

## Monorepo Best Practices

✅ Always prefix with `[App:name]` or `[Pkg:name]`
✅ Use correlation IDs for cross-app requests
✅ Log all inter-app communication
✅ Include package versions on init
✅ Centralize logging in shared package
✅ Sanitize sensitive data

❌ Don't log without app/package context
❌ Don't mix log formats across apps
❌ Don't log passwords/tokens
❌ Don't log in tight loops

## Cross-App Tracing

```javascript
// Generate correlation ID in api
const correlationId = generateId();

// Pass to web
await fetch("web/api", {
	headers: { "X-Correlation-ID": correlationId },
});

// Log everywhere
console.info("[App:api]: Request | correlationId:", correlationId);
console.info("[Pkg:utils]: Processing | correlationId:", correlationId);
console.info("[App:web]: Received | correlationId:", correlationId);
```

## Shared Logger

There is no shared logger package yet. When a second consumer needs one, add it to `packages/utils/src/` and export it from `@open-sunsama/utils`:

```typescript
export const createLogger = (appOrPackageName: string) => ({
	error: (source, operation, error, context) => {
		const prefix = appOrPackageName.startsWith("@")
			? `[Pkg:${appOrPackageName}]`
			: `[App:${appOrPackageName}]`;
		console.error(`${prefix} [${source}] [${operation}]: ${error.message}`, {
			error,
			...context,
		});
	},
	// ... other methods
});

// Usage
import { createLogger } from "@open-sunsama/utils";
const logger = createLogger("api");
logger.error("Service", "operation", error, { userId });
```

## Sensitive Data

Never log:

- Passwords/API keys
- Tokens/certificates
- Credit card/SSN
- Session cookies
- Internal credentials

Sanitize:

```javascript
const sanitized = {
	...data,
	password: undefined,
	cardNumber: "****" + data.cardNumber?.slice(-4),
};
console.log("[App:api] [Service]: Data", sanitized);
```

## Production

- Use shared logging package
- Add correlation IDs
- Per-app log levels
- Centralize with tags (Datadog, Sentry)
- Track cross-app metrics
