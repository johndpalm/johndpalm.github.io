# Plan: Remove Sass `@import` warnings (Minima → Sass modules)

This site uses the Minima theme, which still relies on Sass `@import`. Dart Sass deprecates `@import` and will remove it in 3.0. Today, the only remaining warning in our build comes from `@import "minima"` in `assets/main.scss`.

This document lays out options and a concrete, low-risk path to eliminate those warnings.

## Options

1) Vendor Minima SCSS and convert to modules
- Copy Minima’s `_sass/minima` partials into our repo.
- Replace `@import` with `@use`/`@forward`, and export a single entry partial we can consume from `assets/main.scss`.
- Pros: Fully under our control; no waiting on theme updates; silent build.
- Cons: We own the theme updates going forward; small one-time migration effort.

2) Upgrade or switch theme
- If a newer Minima (or alternate theme) ships Sass modules, upgrade.
- Pros: Upstream maintained.
- Cons: Design drift or breaking changes possible; depends on theme roadmap and Pages constraints.

Recommendation: Option 1 (vendor + module-ize). It’s predictable and keeps the current look and layouts intact.

## Proposed Implementation (Option 1)

High-level:
- Create `_sass/theme/` to host vendored Minima SCSS.
- Build a `theme/_index.scss` that `@forward`s Minima partials.
- Update `assets/main.scss` to `@use "theme"` instead of `@import "minima"`.
- Keep our custom styles in `assets/main.scss` after the theme load.

Step-by-step:
1. Vendor SCSS
- Copy Minima SCSS from the installed gem (or its repo) into `_sass/theme/minima/`.
  - Typical source path inside gem: `$(bundle show minima)/_sass/minima/*`
  - Resulting files go to: `_sass/theme/minima/*.scss`

2. Create a module entry point
- Add `_sass/theme/_index.scss` that re-exports the vendored theme pieces using `@forward`.
  - Example:
    - `@forward "minima/variables";`
    - `@forward "minima/base";`
    - `@forward "minima/layout";`
    - `@forward "minima/syntax-highlighting";`
    - Adjust list to match actual files present.

3. Switch from `@import` to `@use`
- In `assets/main.scss`, remove `@import "minima"`.
- Add `@use "sass:color";` (already present) and `@use "theme";` at the top.
- Ensure any variable overrides happen via `@use ... with (...)` if needed; otherwise keep custom styles after the `@use`.

4. Verify variable access
- With modules, variables are namespaced. If we rely on Minima variables (e.g., `$brand-color`), prefer to keep custom styles that reference them after `@use "theme";`.
- If Minima exports variables, they’ll be available in the global scope only if explicitly forwarded as such; otherwise reference with namespace (e.g., `theme.$brand-color`). Decide one approach and apply consistently.

5. Update config (optional)
- Jekyll already adds `_sass/` to the Sass load paths. If needed, add:

```yaml
sass:
  load_paths:
    - _sass
```

6. Build and test
- `bundle exec jekyll build` locally.
- Ensure there are no `@import` warnings and visual parity is retained.

## Acceptance Criteria
- No Sass `@import` deprecation warnings in build output.
- Visual parity with current site (header, footer, posts, code blocks, homepage grid).
- All pages build successfully on GitHub Actions and render correctly on Pages.

## Rollback
- Keep the previous `assets/main.scss` `@import "minima"` line in git history. If something looks off, revert to the prior commit.

## Notes
- We already use modern Sass APIs (`@use "sass:color";` and `color.adjust`) in our custom styles.
- After migration, we can selectively prune unused Minima partials if desired to speed up compilation.
