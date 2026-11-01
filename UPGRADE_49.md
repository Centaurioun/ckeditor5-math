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

## Hosted CI evidence (2026-11-01)

- Tested code revision: `208d9a79546c050cba42b9d513c7307d2a42d748`.
- GitHub Actions [PR run 41331051217](https://github.com/Centaurioun/ckeditor5-math/actions/runs/41331051217): **SUCCESS**, both Node 22 and Node 24 compatibility jobs.
- TypeScript typecheck, `esbuild` bundle, and declaration build: PASS on both Node versions.
- Real Chromium/Playwright smoke on Node 22: **2/2 passed**, covering actual CKEditor startup/math toolbar and inline/block math data round-trip with KaTeX render.
- No merge to `master`; PR #1 remains draft pending deeper compatibility acceptance.

### Not yet verified by this small smoke suite

- Original CKEditor 43-era test files under `tests/` have **not** been ported to and executed on CKEditor 49. They are not included in the claimed 2/2 result.
- The smoke does not fully exercise equation dialog keyboard interactions, existing-equation edit/cancel, paste/autoformat, cross-browser behavior, malformed TeX security, Word/HTML import, or ScientificRadar Studio integration.
- Legacy `yarn.lock` describes the old 43-era dependency tree. CI currently installs the pinned 49.0.1 graph without a committed fresh lockfile; a new lockfile should be generated and reviewed before a production/published release.
- The fork package is marked `private: true`, and no npm release was published. Verify plugin-specific ISC and CKEditor GPL/commercial licensing independently when integrating.
