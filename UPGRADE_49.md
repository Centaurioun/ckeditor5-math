# CKEditor 5 v49.0.1 compatibility migration

This is an **unmerged, unreleased** migration candidate in Centaurioun's [fork](https://github.com/Centaurioun/ckeditor5-math) of isaul32/ckeditor5-math (ISC licence).

- Baseline upstream plugin: v43.2.0 and legacy `ckeditor5/src/*` imports.
- Target editor: the stable **CKEditor 5 v49.0.1**, not v49.1 alpha/nightly.
- New import mechanism: `ckeditor5` package (NIM); do not mix legacy imports into live plugin.
- The old DLL toolchain and legacy `ckeditor5-package-tools test` runner were **removed from scripts**, but their historical test sources remain under `tests/`. Until real browser tests are ported, **a passing CI only confirms TypeScript and bundler compatibility**.
- New content defaults to non-executable span-based TeX output; legacy script math is still recognized. Untrusted equation fallback must be written using `textContent` and KaTeX trust disabled.
- Peer dependency `ckeditor5 ^49.0.1`; the fork is intentionally marked `private` to avoid accidental npm publication.
- The old `yarn.lock` is CKEditor 43-era and **must not be used as the v49 lock**; new CI uses a fresh exact-version npm installation, pending committed lock generation.

## Local gates

```sh
npm install --ignore-scripts --no-audit --no-fund --package-lock=false
npm run typecheck
npm run build:smoke
npm run build
```

## Still needed for full acceptance

Restore/up-port functional tests to current CKEditor APIs, run a real browser/editor smoke: inline/display insertion, edit existing TeX, cancel, KaTeX fonts/preview, bad equation, safe export, round-trip and undo. Test inside the intended Next.js integration separately; do not change ScientificRadar Studio in this PR.

Update guides:
- https://ckeditor.com/docs/ckeditor5/latest/updating/nim-migration/migration-to-new-installation-methods.html
- https://ckeditor.com/docs/ckeditor5/latest/updating/guides/update-to-49.html
