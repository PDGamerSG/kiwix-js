# Validation for Kiwix JS #568 / draft PR #1574

PR head: `8acdd9a6822a4edce0440cf534c0edb82a3a5f27`. Base: `e809ccd993cca12ded4dfc40b9ad79233ed71a19`.

Checks were run by Codex on macOS using the existing repository tests. Browser suites used freshly built production files and the bundled Ray Charles (XZ), Gutenberg Romanian (ZSTD), and Tonedear fixtures in Restricted and Service Worker modes.

| Check | Result |
| --- | --- |
| `npm test` | 94 passing |
| `npx eslint` | Exit 0 |
| `npm run build` | Exit 0; regular and minified bundles built |
| `git diff --check` | Exit 0 |
| JSDoc scope check | Only JSDoc blocks changed in all 10 files |
| Babel executable AST comparison | Unchanged in all 10 files |
| Base/head production build comparison | Minified app bundle and service worker byte-identical with the same dependencies |
| `GITHUB_ACTIONS=true CI=true npm run test-e2e-firefox` | 48 passing, 4 pending |
| `GITHUB_ACTIONS=true CI=true npm run test-e2e-chrome` | 42 passing, 4 pending, 6 failing |
| Same Chrome command on base `e809ccd` | 42 passing, 4 pending; the same 6 tests fail |
| `GITHUB_ACTIONS=true CI=true npm run test-e2e-extension-firefox` | 2 passing |
| `GITHUB_ACTIONS=true CI=true npm run test-e2e-extension-chrome` | 3 passing, using Chrome for Testing |

The six Chrome web failures concern the Ray Charles Service Worker popover/search, Gutenberg Service Worker language switching, and Tonedear navigation/images. See `chrome-baseline-comparison.log` and the full head/base browser logs. These failures were reproduced without the annotation changes; they were not repaired as part of this JSDoc-only PR.

Firefox used version 160.0.1. Chrome web runs used 158.0.8127.36 (head) and 158.0.8127.37 (base); the Chrome extension run used Chrome for Testing 158.0.8127.36. See the raw logs for available capability details.

The 27 annotated functions were also reviewed directly against their return/resolve paths in [type-audit.md](type-audit.md). Automated runtime tests do not validate documentation types.

The two PNGs are genuine GitHub Files changed screenshots showing the old annotations in red and the corrected annotations in green. Screenshot assets and logs are on this separate evidence branch; the PR itself contains only JSDoc changes. Local checkout paths in uploaded logs have been replaced with placeholders for readability. Original logs remain in the local evidence folder.

This PR remains a draft for the author's review. No author personal-testing or understanding attestations are made by these automated checks.
