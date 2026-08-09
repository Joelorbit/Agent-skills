---
name: frontend
description: >-
  Frontend engineering standard covering UI states, component design,
  accessibility (a11y), responsive layouts, and performance. Activate when building UI features.
---

# Frontend Engineering Standard

## 1. UI State Coverage
Never design or implement a user interface that only handles the "happy path." Every component interacting with network data or user inputs must account for six states:

| UI State | Target Behavior | Requirement |
| :--- | :--- | :--- |
| **Loading** | Indicated progress | Skeleton loaders or spinners. Avoid layout shifts. |
| **Empty** | No items to show | Friendly message with an action button (e.g., "Create project"). |
| **Error** | Actionable failure | Non-technical description, error logs mapped, retry button. |
| **Success** | Successful fetch/save | Render complete data. Show clear confirmation for mutations. |
| **Partial Data** | Partial rendering | Degrade gracefully if some columns or cards fail to load. |
| **Offline** | Network drop | Banners informing the user. Disable state mutations. |

## 2. Component Design & Composition
- **Separation of Concerns:** Separate data-fetching containers from presentation-only UI components.
- **Single Responsibility:** A component should do one logical job. Break down components larger than 250 lines.
- **Props-Driven:** Component behaviors should be predictable and configurable via explicit, strongly-typed props.

## 3. Responsive Layouts & Styling
- **Mobile First:** Design layouts for small devices first, then apply media queries for larger displays.
- **No Hardcoded Breakpoints:** Use logical layouts (flexbox, grid) and standardized media queries.
- **Touch Targets:** Interactive elements (buttons, links, form inputs) must have a minimum touch target size of 44x44px.
- **Overflow Prevention:** Ensure text breaks properly. Never allow content to cause horizontal scrolling on viewport widths >320px.

## 4. Accessibility (a11y) Rules
Accessibility is a measure of code correctness, not an optional feature.
- **Semantic HTML:** Use native elements (`<button>`, `<a href>`, `<nav>`, `<main>`, `<article>`) instead of divs with custom click handlers.
- **Keyboard Navigation:** Every interactive element must be reachable using `Tab` and triggerable using `Enter` or `Space`.
- **Focus Outlines:** Never remove focus outlines (`outline: none`) without providing a custom, high-contrast focus indicator.
- **Labels:** Every input element must be programmatically associated with a `<label>`. Use `aria-label` or `aria-labelledby` where visual labels are not possible.
- **Contrast:** Ensure all text passes WCAG AA contrast rules (minimum 4.5:1 for normal text, 3:1 for large text).
- **Reduced Motion:** Respect system settings by disabling non-essential transitions using `prefers-reduced-motion` media queries.

## 5. Frontend Performance
- **Image Optimization:** Always use responsive image sizes (`srcset`) and next-gen formats (WebP, AVIF). Add explicit height/width to prevent layout shifts (CLS).
- **Lazy Loading:** Dynamically import components, routes, and below-the-fold images.
- **Bundle Control:** Keep bundle sizes low. Tree-shake libraries. Prefer lightweight native APIs over massive npm dependencies.

## 6. Design System: EyuTheme (MANDATORY)

All frontend work uses the **EyuTheme** design system (`Joelorbit/Mytheme`). Do not invent ad-hoc colors, fonts, radii, or spacing. Copy the design tokens and adapt components to the target framework; never redesign the theme.

- **Token Contract:** `src/lib/tokens.css` is the single source of truth. No hex/oklch values outside it — reference tokens by CSS variable.
- **Palette:** Neutral oklch ramp (`--n-50` → `--n-1000`), blue-violet accent (`--accent: oklch(0.52 0.17 265)`), gold complement (`--complement: oklch(0.65 0.14 60)`), status colors (`--success/--warning/--danger`).
- **Typography:** `--font-display: 'Outfit'`, `--font-body: 'Lexend'`, `--font-mono: 'JetBrains Mono'`. Use the 1.333x type scale tokens (`--display-lg` → `--caption`) with matching line-heights and tracking.
- **Dark Default:** Dark is the default (`:root`). Light mode via `[data-theme='light']` — re-theme by overriding the neutral ramp only, never individual components.
- **Spacing:** 8pt grid (`--space-0` → `--space-12`). Radius `--radius-*`, shadows `--shadow-1..3`, motion `--dur-*` / `--ease-*` from tokens.
- **Texture:** Fine digital-noise grain (`--grain-svg` / `--grain-opacity`) and plus pattern are part of the brand — apply subtly, never overdo.
- **Components:** Reuse existing UI components (Button, Card, Badge, Dialog, Input, Table, Skeleton, EmptyState, Select, Separator, MicroLabel, BookingCard, ThemeToggle) from `src/lib/components/ui/`. Composition over inheritance: slots + snippets, thin extensible components, state in / callbacks out (e.g., `Dialog` takes `open` + `onClose`).
- **Naming:** BEM-ish `block__element--modifier`. Svelte 5 + Tailwind 4. No duplicated styles: one component, one file, one concern.

**Rule:** if a design decision isn't in the tokens, extend `tokens.css` following the existing ramp/mix patterns — never scatter raw values through components.
