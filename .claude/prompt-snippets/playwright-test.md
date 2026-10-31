# Playwright E2E Testing

Validate scenarios using Playwright for end-to-end testing.

## Usage

```
/playwright-test "User can log in and see dashboard"
/playwright-test e2e/auth.spec.ts
````

## Process

1. **Check for Existing Test**:
   - Look in `e2e/` for matching scenario
   - If found, run the existing test

2. **Create New Test** (if needed):
   - Create test file in `e2e/`
   - Follow project testing conventions
   - Include proper setup and teardown

3. **Run Test**:
   ```bash
   bun run e2e
   # or
   npx -y playwright test <test-file>
````

4. **Report Results**:
   - Summarize test outcomes
   - Report any failures with details
   - Suggest fixes for failing tests

## Test Template

```typescript
import { test, expect } from "@playwright/test";

test.describe("{{FEATURE_NAME}}", () => {
	test("should {{TEST_DESCRIPTION}}", async ({ page }) => {
		await page.goto("http://localhost:3000");

		// Test steps here

		await expect(page).toHaveURL(/expected-path/);
	});
});
```

## Configuration

- Test directory: `e2e/`
- Base URL: `http://localhost:3000`
- Run command: `bun run e2e`
