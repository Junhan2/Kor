# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Fancy Components is an animated React component library designed to make the web fun again. It provides a shadcn/ui-style registry system where users can install components via CLI (`npx shadcn add @fancy/component-name`).

**Tech Stack:**
- Next.js 16.0.1 (App Router + Turbopack as default)
- React 19.1.1
- Motion 12.23.24 (use `motion/react`, NOT `framer-motion`)
- Tailwind CSS v4
- TypeScript (strict mode)
- MDX for documentation
- KaTeX for mathematical expressions

## Common Commands

```bash
# Development
pnpm dev                    # Start dev server (Turbopack enabled by default)
pnpm build:registry         # Rebuild registry only (index + sources)
pnpm build                  # Full production build (registry + docs + llms + Next.js)
pnpm lint                   # Run ESLint
pnpm format                 # Format code with Prettier
pnpm format:check           # Check formatting

# Registry build pipeline (runs in order during pnpm build)
pnpm build:index            # Generate registry index from source files
pnpm build:source           # Generate individual component JSON files
pnpm build:docs             # Convert MDX to markdown
pnpm build:llms             # Generate LLM-friendly text docs
```

**Note:** Next.js 16 uses Turbopack by default. No `--turbopack` flag needed.

**Important:** Always run `pnpm build:registry` after modifying components, hooks, or examples. The registry must be rebuilt for documentation to work correctly.

## Architecture

### Registry System (Core Pattern)

This project implements a shadcn/ui-style component registry with automatic dependency inference.

**Key Directories:**
```
src/fancy/
├── components/         # Component source code (organized by category)
│   ├── text/
│   ├── blocks/
│   ├── carousel/
│   ├── background/
│   ├── physics/
│   ├── image/
│   └── filter/
├── examples/           # Component demos (same category structure)
├── schema.ts           # Zod schema for registry
└── index.ts            # Generated registry index (gitignored)
```

**Build Pipeline:**

1. **`build:index`** (`src/scripts/build-registry-index.ts`)
   - Scans source files and infers dependencies by parsing imports
   - Detects hooks (`@/hooks/*`), utils (`@/utils/*`), components, and external packages
   - Generates `src/fancy/index.ts` (TypeScript registry)
   - Generates `public/r/registry.json` (shadcn-compatible JSON)

2. **`build:source`** (`src/scripts/build-registry-sources.ts`)
   - Creates individual JSON files per component in `public/r/{name}.json`
   - Transforms import paths for v0 compatibility
   - Converts Tailwind color classes to hex values

3. **`build:docs`** + **`build:llms`**
   - Convert MDX documentation to markdown and LLM-friendly formats

**Critical Import Rules:**

The registry build script only recognizes specific import patterns. Always use:

```tsx
// ✅ CORRECT - Registry will detect these
import { useMouse } from "@/hooks/use-mouse-position"
import { cn } from "@/utils/cn"
import Component from "@/fancy/components/text/foo"

// ❌ WRONG - CLI installation will fail
import { useMouse } from "../../hooks/use-mouse-position"
import { cn } from "../utils/cn"
```

**Component Configuration (Optional):**

Create a `.json` file alongside the component to specify additional metadata:

```
src/fancy/components/blocks/my-component.tsx
src/fancy/components/blocks/my-component.json  ← Optional config
```

Example config (`src/fancy/components/blocks/circling-elements.json`):
```json
{
  "additionalDependencies": ["package-name"],
  "devDependencies": ["@types/package"],
  "cssVars": {
    ":root": {
      "--my-color": "#ffffff"
    }
  },
  "tailwind": {
    "config": {
      "theme": {
        "extend": {}
      }
    }
  }
}
```

### Documentation System

**Structure:**
```
src/content/docs/
├── components/{category}/{name}.mdx
├── introduction.mdx
├── installation.mdx
└── changelog.mdx
```

**MDX Frontmatter:**
```yaml
---
title: Component Name
description: Brief description
component: true
author: name <https://example.com>
---
```

**Special MDX Components:**
- `<ComponentPreview name="demo-name" />` - Live demo
- `<ComponentSource name="component-name" />` - Source code display
- `<InstallTabs command="..." />` - Installation instructions
- `<CodeSnippet title="...">` - Code blocks

**Navigation:** Update `src/config/docs.ts` to add components to sidebar.

### Next.js App Router

```
src/app/
├── page.tsx                    # Home page
├── docs/                       # Documentation routes
├── components/                 # Component showcase routes
├── globals.css                 # Tailwind v4 styles + CSS variables
└── api/
    ├── [...slug]/route.ts      # Serve pre-built markdown
    └── revalidate/route.ts     # ISR revalidation
```

## Component Development Workflow

### 1. Create Component Source

```bash
# Location: src/fancy/components/{category}/{name}.tsx
```

**Requirements:**
- Add author comment at top: `// author: name <https://url>`
- Use only `@/hooks` and `@/utils` import aliases
- Follow tech stack: React 19, Motion 12 (`motion/react`), Tailwind v4
- **CRITICAL:** Use `import { motion } from "motion/react"` NOT `"framer-motion"`
- Optimize performance: use MotionValues, `useAnimationFrame` over `useEffect`

### 2. Create Component Demo

```bash
# Location: src/fancy/examples/{category}/{name}-demo.tsx
```

Multiple demos are allowed for different variations.

### 3. Rebuild Registry

```bash
pnpm build:registry
```

### 4. Write Documentation

```bash
# Location: src/content/docs/components/{category}/{name}.mdx
```

**Required Sections:**
1. Component preview (first)
2. Installation (CLI + Manual tabs)
3. Usage / Understanding the component
4. Examples (if applicable)
5. Notes (if applicable)
6. Props table
7. Credits (if applicable)

### 5. Update Navigation

Add entry in `src/config/docs.ts` under the appropriate category:

```ts
{
  title: "Component Name",
  href: "/docs/components/{category}/{name}",
  label: "New"
}
```

## Code Conventions

### Motion Library Imports

**CRITICAL:** Always use the correct Motion import pattern:

```tsx
// ✅ CORRECT - Use motion/react
import { motion, AnimatePresence, useMotionValue } from "motion/react"

// ❌ WRONG - Never use framer-motion
import { motion } from "framer-motion"
```

The project uses the `motion` package (Motion v12), which is the successor to `framer-motion`. Using the wrong import will cause build failures.

### Author Attribution

**Required in all files:**
```tsx
// author: daniel petho <https://www.danielpetho.com>
// author: john doe <https://example.com>, jane smith <https://example.org>
```

### Import Order (Prettier)

1. `react`
2. `next`
3. External libraries
4. `@/types`
5. `@/config`
6. `@/lib`
7. `@/hooks`
8. `@/components/ui`
9. `@/components`
10. `@/fancy`
11. `@/styles`
12. `@/app`
13. Relative imports

### Import Aliases

```json
{
  "@/*": ["./src/*"]
}
```

### Tailwind v4 Custom Colors

See `src/app/globals.css`:
```css
--color-primary-red: #ff5941
--color-primary-orange: #f97316
--color-primary-pink: #e794da
--color-primary-blue: #0015ff
--color-teal: #1f464d
```

## Generated Files (gitignored)

```
src/fancy/index.ts          # TypeScript registry
public/r/                   # JSON registry files
public/docs/                # Markdown documentation
```

Never manually edit these files. They are regenerated by build scripts.

## Component Quality Standards

**Performance:**
- Use Motion MotionValues where applicable (`import { useMotionValue } from "motion/react"`)
- Prefer `useAnimationFrame` from Motion over `useEffect` for animations
- Minimize re-renders
- Test with [react-scan](https://github.com/aidenybai/react-scan)

**Accessibility:**
- Use `sr-only` for screen reader content
- Apply `aria-hidden` to decorative elements
- Reference existing text components for examples

**Creativity:**
- Prioritize unconventional, creative components over pure utility
- Demos should be visually engaging and responsive across viewports

**Attribution:**
- No 1:1 copies without permission
- Credit original creators for specific inspirations
- Include credits in documentation and under demos

## Commit Convention

Format: `category(scope): message`

**Categories:**
- `feat` / `feature` - New features or code
- `fix` - Bug fixes
- `refactor` - Code changes (not fix or feature)
- `docs` - Documentation changes
- `build` - Build system or dependencies
- `test` - Test changes
- `ci` - CI configuration
- `chore` - Other changes

Example: `feat(components): add gravity text effect`

## Important Files Reference

- **Registry Schema:** `src/fancy/schema.ts`
- **Build Scripts:** `src/scripts/build-registry-*.ts`
- **Navigation Config:** `src/config/docs.ts`
- **Contribution Guide:** `CONTRIBUTING.md`
- **Acceptance Criteria:** `ACCEPTANCE_CRITERIA.md`

## shadcn CLI Integration

Users install components via:
```bash
npx shadcn add @fancy/component-name
```

Registry configuration in `components.json`:
```json
{
  "registries": {
    "@fancy": "https://fancycomponents.dev/r/{name}.json"
  }
}
```

## v0 Compatibility

Build scripts automatically transform import paths for Vercel v0:
```tsx
// Source
import Component from "@/fancy/components/text/foo"

// Transformed in registry JSON
import Component from "@/components/fancy/text/foo"
```

## Next.js 16 Specific Information

### Breaking Changes Applied

The project has been upgraded to Next.js 16.0.1. Key changes:

**1. Turbopack is now default:**
- No need for `--turbopack` flag in dev/build commands
- Turbopack is automatically enabled
- Use `--webpack` flag only if webpack is explicitly needed

**2. revalidateTag API signature changed:**
```tsx
// ✅ CORRECT - Next.js 16
import { revalidateTag } from "next/cache"
revalidateTag("tag-name", "max")  // Second argument required

// ❌ WRONG - Old Next.js 15 syntax
revalidateTag("tag-name")  // Will cause TypeScript error
```

**3. Async Request APIs:**
- All `params`, `searchParams` already use `await` (no changes needed)
- Project is already compliant with Next.js 16 requirements

**4. No middleware.ts:**
- Project doesn't use middleware
- No migration to proxy.ts needed

### Performance Improvements

Next.js 16 with Turbopack provides:
- **5-10x faster** Fast Refresh during development
- **2-5x faster** production builds
- **Sub-second** dev server startup (typically < 500ms)

## Notes

- Contentful CMS is used for component thumbnails and demo videos (requires env vars)
- Vercel Analytics and Speed Insights are enabled
- Package manager: **pnpm** (not npm)
- Always rebuild registry after component changes
- Test CLI installation locally before submitting PRs
- **Next.js 16.0.1** with Turbopack as default bundler
