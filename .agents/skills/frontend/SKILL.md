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
