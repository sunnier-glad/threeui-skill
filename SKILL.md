---
name: threeui
description: Build React interfaces with the same interactive component-catalog experience as ThreeUI: browseable entries, search, routing, themes, live previews, variants, controls, and source/install documentation. Use for ThreeUI-like catalogs or when the user explicitly asks to reproduce the ThreeUI experience; do not use for generic React UI work.
metadata:
  short-description: Build ThreeUI-style interactive catalogs
---

# ThreeUI-style catalog builder

Use the public ThreeUI project as the behavioral and visual reference when the user asks for a ThreeUI-like experience. The target is a usable component catalog and preview application, not a static marketing mockup.

## Reference and boundaries

- Public reference repository: <https://github.com/MengTo/threeui>
- Public package: `@designcodeio/threeui`
- Community catalog: <https://threeui.com>
- Read [references/threeui-catalog.md](references/threeui-catalog.md) when building the catalog shell, registry, routes, or preview controls.

Reproduce the public interaction model and information architecture while adapting branding, content, and layout details to the user's product. Do not copy private or Pro/Beta source, protected assets, remote thumbnails, or brand identity without permission. Preserve required license and third-party notices when reusing MIT Community code or assets.

## Decide the task mode

Choose the smallest mode that fulfills the request:

1. **Catalog application** — build or extend the full browse/search/preview experience described below.
2. **Component integration** — use verified components from `@designcodeio/threeui` inside an existing React app, while keeping the host app's structure.
3. **Single preview or page** — implement one verified component or renderer with its controls and responsive behavior; do not scaffold the whole catalog unless requested.

If the user does not specify a framework, prefer React + TypeScript with the host project's existing build tool. Do not replace an existing stack without a reason.

## Full catalog behavior

For catalog-application tasks, implement these behaviors as a coherent system:

- A top bar with menu access, a compact brand mark, an upgrade/action area when relevant, and theme controls.
- A responsive sidebar/navigation rail listing catalog categories or featured entries, plus Browse, installation documentation, and any product-specific documentation routes.
- A browse page with a metadata-driven card grid, category filters, tag filters, descriptions, thumbnails/previews, and empty states.
- Search in a dialog or command palette. Support `Ctrl/Cmd+K` to open it and `Escape` to close overlays. Search names, descriptions, categories, and tags.
- Detail pages with a live preview, loading/error states, title and description, runtime/interaction information, variant selection, controls, source/import tabs, and installation guidance.
- URL-addressable routes for browse filters, catalog entries, variants, and documentation. Keep browser back/forward behavior working; use the project's router if present, otherwise a small route layer with `history.pushState`/`popstate` is acceptable.
- Light, dark, and system appearance modes where they fit the product. Persist the choice and any user-selected palette safely, but keep a usable default when storage is unavailable.
- A narrow-screen layout: collapsible navigation, scrim/overlay dismissal, readable controls, no horizontal overflow, and touch-sized interactive targets.

Do not make cards decorative only: clicking a card must reach a working detail/preview route, and controls must change the rendered preview or clearly report when a renderer does not expose that capability.

## Public component integration

When using the public package:

1. Inspect the existing package manager and React/TypeScript setup.
2. Install the package only when needed:

   ```bash
   npm install @designcodeio/threeui
   ```

   Use the project's package manager when it differs from npm.
3. Import shared styles once and prefer verified component subpaths when supported:

   ```tsx
   import { AtTheHorizon } from "@designcodeio/threeui";
   import "@designcodeio/threeui/style.css";

   export function Hero() {
     return <AtTheHorizon />;
   }
   ```

   ```tsx
   import { AtTheHorizon } from "@designcodeio/threeui/components/AtTheHorizon";
   ```

Never invent component names, props, variants, or asset URLs. Verify them from installed exports, package types, the public repository, or the Community catalog before coding.

## Renderers and assets

Keep catalog metadata separate from renderer code. Use a registry keyed by stable IDs; lazy-load expensive Three.js/WebGL renderers where practical; and keep the active renderer, variant, and control values synchronized with route state.

Some public components render complete HTML documents or use root-relative runtime files. Copy only the required files from `node_modules/@designcodeio/threeui/lib-dist/assets/` into the app's public directory, or use a verified `sourceUrl`/`assetBaseUrl` prop. Test asset resolution from the deployed base path as well as the development root.

For WebGL or browser-only components, follow the host framework's client-component/SSR rules. Provide loading and failure states, dispose renderer resources on unmount, respect reduced motion where possible, and keep essential text and actions available outside the canvas.

## Access and licensing

The public repository and npm package expose the Community implementation. Do not claim Pro or Beta source is public. Do not authenticate, download entitled source, or overwrite project files with the ThreeUI Pro CLI unless the user explicitly requests it and confirms access. The documented command is:

```bash
npx @designcodeio/threeui-cli add <component-slug>
```

Do not redistribute remote catalog thumbnails or previews as local assets without checking their license. Keep MIT, SIL Open Font License, and third-party notices with any reused material.

## Verification

For a catalog application, run the available type check, lint, tests, and production build. Then verify:

- Browse, category/tag filters, search, detail routes, variants, and documentation links.
- Direct loading and browser back/forward for representative URLs.
- Light/dark/system themes and persistence.
- Mobile navigation, keyboard shortcuts, focus order, labels, and reduced-motion behavior.
- Preview loading/error states, WebGL cleanup, asset URLs, console errors, and horizontal overflow.
- At least one real renderer and one renderer with controls, rather than only mocked cards.

Report unverified APIs, missing assets, unavailable WebGL, or environment limitations instead of presenting a visual placeholder as complete functionality.
