---
paths:
  - "apps/web/src/**/*.{tsx,jsx}"
  - "apps/desktop/src/**/*.{tsx,jsx}"
  - "apps/mobile/src/**/*.{tsx,jsx}"
  - "apps/expo-mobile/app/**/*.{tsx,jsx}"
  - "apps/expo-mobile/src/**/*.{tsx,jsx}"
  - "packages/**/components/**"
description: "Component structure and composition patterns."
---

# Component Structure

Follow the rules defined in [.claude/rules-snippets/component-structure.md](../rules-snippets/component-structure.md).

Key points:
- Follow the function-component pattern with typed props, and Radix UI primitives for behavior, for all components.
- Use clear and descriptive naming for props, events, and methods.
- Add code comments for complex logic or non-obvious implementations.
- Use bracketed identifiers for logs: `[ComponentName]: message`.
