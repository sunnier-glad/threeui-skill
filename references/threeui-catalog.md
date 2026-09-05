# ThreeUI catalog reference

Use this reference only when building or substantially extending a ThreeUI-style catalog application.

## Information architecture

The public application is organized around these page states:

- `browse`: all catalog entries, optionally filtered by category or tag
- `shader`: one entry's live preview and documentation
- `installation`: setup and package guidance
- `mcp`: product-specific integration documentation when applicable
- `not-found`: a recoverable route error with a link back to Browse

The shell has a top bar, a navigation sidebar, one scrollable main pane, a footer, and an overlay search dialog. On mobile, the sidebar becomes a drawer with a dismissible scrim.

## Registry model

Keep the registry data-driven. A useful entry shape is:

```ts
type CatalogEntry = {
  id: string;
  variantOf?: string;
  category: string;
  label: string;
  thumbnail?: string;
  preview?: string;
  tags: string[];
  description: string;
  runtime: string;
  origin?: string;
  sourceCommit?: string;
  sourceFiles?: string[];
  passes?: string;
  interaction?: string;
  asset?: string;
  assetCount?: number;
  importName?: string;
  contract?: ContractRow[];
  controls?: Control[];
  variants?: Variant[];
};

type ContractRow = {
  name: string;
  type: string;
  value: string;
};

type Variant = {
  id: string;
  label: string;
  description: string;
  thumbnail?: string;
  preview?: string;
  props?: Record<string, boolean | number | string>;
  controls?: Control[];
};
```

Supported control kinds from the public model are:

- `range`: numeric min, max, step, and display precision
- `choice`: labeled options
- `checkpoint`: discrete options used for checkpoints or presets
- `color`: color value
- `text`: text value, optionally with max length and placeholder

Treat this as a design model, not a reason to fabricate metadata. Only expose fields that the actual renderer supports.

## Interaction contract

Use one source of truth for route state. Selecting an entry should:

1. resolve a base entry and optional variant;
2. update the active preview and controls;
3. push or replace the canonical URL;
4. close search and mobile navigation overlays;
5. restore the same state when the URL is loaded directly or browser history changes.

Selecting a category or tag should reset the incompatible filter and produce a stable URL. Search should select an entry through the same route transition as the sidebar and browse cards.

Theme state should support `light`, `dark`, and `system` when the product needs them. Apply the resolved scheme to the document, listen for system-scheme changes in system mode, and tolerate unavailable `localStorage`.

## Preview layout

Detail pages should make the renderer the primary visual surface. Put explanatory metadata and controls around it without covering essential interactions. Include:

- a visible loading state while a lazy renderer resolves;
- an error boundary or recoverable error state;
- a variant picker only when variants exist;
- controls generated from the entry/variant metadata;
- source/import information that is copyable and verified;
- an installation section appropriate to the selected package/framework.

For document-style renderers, isolate the document preview so its CSS and runtime do not leak into the catalog shell. For canvas renderers, size the canvas from its container, handle resize and device pixel ratio, and clean up listeners/animation loops on unmount.

## Visual direction

Aim for a restrained, editorial developer-tool interface: strong typographic hierarchy, quiet surfaces, compact navigation, generous preview space, clear metadata, and motion used to communicate state. Keep the preview and its controls visually dominant. Adapt colors and branding to the user's product instead of copying ThreeUI's mark, exact text, or remote artwork.

## Minimal acceptance scenario

Before calling the catalog complete, manually exercise this path:

1. Open Browse on a narrow viewport.
2. Open search with `Ctrl/Cmd+K`, search for a verified entry, and select it.
3. Change a variant and at least one control.
4. Reload the detail URL and use browser Back to return to Browse.
5. Toggle theme, close/reopen the mobile drawer, and verify no console error or horizontal overflow.
