---
applyTo: "**/*"
description: "Component structure patterns and naming conventions for the open-sunsama monorepo."
---

# Component Structure

Follow the rules defined in [.claude/rules-snippets/component-structure.md](../../.claude/rules-snippets/component-structure.md).

Key points:
- Follow the function-component pattern with typed props, and Radix UI primitives for behavior, for all components with clear, descriptive naming for props, events, and methods.
- Keep logic within components focused and manageable; add code comments for complex or non-obvious implementations.
- Use bracketed identifiers for logs: `[ComponentName]: message`.
- Share reusable components via `packages/` when used across multiple apps.
