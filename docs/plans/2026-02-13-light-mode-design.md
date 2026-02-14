# Light Mode Design

**Date:** 2026-02-13
**Status:** Approved

## Approach

CSS custom property override via `[data-theme="dark"]` on `<html>`. Light mode is the default. Dark mode activates when the user toggles. Preference persists via `localStorage('theme')` and syncs across both pages.

## Light Mode Color Palette

| Variable | Dark Value | Light Value |
|----------|-----------|-------------|
| --c-bg | #0a0a0f | #f8f8fa |
| --c-bg-elevated / --c-surface | #12121a | #ffffff |
| --c-surface / --c-surface-alt | #1e1e2a | #f0f0f5 |
| --c-bg-card | #18182299 | #ffffff99 |
| --c-border | #2a2a3a | #d8d8e0 |
| --c-text | #e8e6e3 | #1a1a2e |
| --c-text-muted / --c-text-secondary | #8a8a9a | #5a5a70 |
| --c-text-dim | #5a5a6a | #9090a0 |
| --c-okta-blue | #0066cc | #0052a3 |
| --c-okta-blue-light | #3399ff | #0066cc |
| --c-seller-bg | #1a1710 | #fef9f0 |
| --c-seller-border | #3d3520 | #e0d5c0 |
| Shadows | rgba(0,0,0,...) | rgba(0,0,0,0.06-0.1) |

## Toggle

- Default: Light mode (`:root` has light values)
- Dark mode: `html[data-theme="dark"]` overrides variables to original dark values
- localStorage key: `theme` ("light" or "dark")
- Icon: Sun in dark mode, Moon in light mode
- Placement: Nav bar (index.html), header (technical.html)
- Code blocks stay dark in both themes

## Files Affected

- index.html — `:root` light values, `[data-theme="dark"]` block, toggle button + JS
- technical.html — same pattern
