# Fork maintenance notes

Recorded maintenance baseline: 2026-10-24. This is a snapshot, not a promise that upstream or advisory data is still current.

- Upstream: [https://github.com/excalidraw/excalidraw](https://github.com/excalidraw/excalidraw)
- Integrated upstream commit: [`1d4dc2c0e6e476911d0760e9c3cf3694f2d43384`](https://github.com/excalidraw/excalidraw/commit/1d4dc2c0e6e476911d0760e9c3cf3694f2d43384)
- Verified maintenance code commit: `b11920262f9e4a04f19bf9a20646ffde021b68be`
- Deployment branch: `master`

This fork deploys the Excalidraw application with a private indexing policy. It is not a publication of every upstream example or documentation site.

## Preserve these fork differences

| Paths | Intent |
| --- | --- |
| `excalidraw-app/index.html, public/robots.txt, public/sitemap.xml, vercel.json` | Keep noindex behavior, the intentionally empty sitemap, deployment output directory, and existing hosting headers. |
| `examples/with-nextjs, examples/with-script-in-browser` | These example trees are intentionally absent from this fork. Do not restore them as a side effect of upstream synchronization. |
| `excalidraw-app/vite.config.mts` | Keep the PWA precache limit that allows current generated application assets to be cached. |
| `packages/excalidraw/tests/interactivity.test.tsx; packages/excalidraw/tests/__snapshots__/MermaidToExcalidraw.test.tsx.snap` | Keep test adaptations for the maintained React/Vitest toolchain; do not hide behavior regressions by bulk snapshot updates. |
| `dev-docs` | This has a separate, older lockfile and is not the deployed application. Review it independently before running or publishing the docs site. |

Follow [AGENTS.md](AGENTS.md) for source changes. Real Mermaid preview rendering, PWA build output, and remote collaboration are different verification scopes; collaboration credentials were not exercised in maintenance.

## Dependency and runtime baseline

Use Yarn 1.22.22 and the application root `yarn.lock`. Recorded versions include React 19.3.1, Firebase 11.10.0, Socket.IO 4.8.4, Mermaid converter 2.3.0, Mermaid 11.17.2, DOMPurify 3.4.17, Vitest 4.1.12 and Vite 6.4.5. Maintenance was tested with Node 24.22.1; the broad upstream Node engine declaration is not a patch-level security guarantee.

Package-specific resolutions retain compatible majors where parents need them (for example minimatch, picomatch and ws). Preserve the converter/Mermaid/KaTeX security fixes together. Do not change the workspace package version solely to silence an audit warning.

The manifest and committed lockfile are authoritative for exact dependency resolution. Review both direct and transitive changes; avoid mixing package managers.

## Resolution policy and removal conditions

| Scoped selection | Current declaration | Reason |
| --- | --- | --- |
| `strip-ansi` | `6.0.1` | Retain the reviewed security/compatibility resolution for this dependency family; do not replace all parent majors with a single version. |
| `websocket-driver` | `^0.7.5` | Retain the reviewed security/compatibility resolution for this dependency family; do not replace all parent majors with a single version. |
| `protobufjs` | `^7.6.3` | Retain the reviewed security/compatibility resolution for this dependency family; do not replace all parent majors with a single version. |
| `@protobufjs/utf8` | `^1.1.1` | Retain the reviewed security/compatibility resolution for this dependency family; do not replace all parent majors with a single version. |
| `dompurify` | `^3.4.17` | Retain the reviewed security/compatibility resolution for this dependency family; do not replace all parent majors with a single version. |
| `lodash-es` | `^4.18.1` | Retain the reviewed security/compatibility resolution for this dependency family; do not replace all parent majors with a single version. |
| `source-map-js` | `^1.2.2` | Retain the reviewed security/compatibility resolution for this dependency family; do not replace all parent majors with a single version. |
| `postcss` | `^8.5.31` | Retain the reviewed security/compatibility resolution for this dependency family; do not replace all parent majors with a single version. |
| `socket.io-parser` | `^4.2.7` | Retain the reviewed security/compatibility resolution for this dependency family; do not replace all parent majors with a single version. |
| `@babel/runtime` | `^7.29.7` | Retain the reviewed security/compatibility resolution for this dependency family; do not replace all parent majors with a single version. |
| `mermaid` | `^11.17.2` | Retain the reviewed security/compatibility resolution for this dependency family; do not replace all parent majors with a single version. |
| `katex` | `^0.18.2` | Retain the reviewed security/compatibility resolution for this dependency family; do not replace all parent majors with a single version. |
| `**/@excalidraw/mermaid-to-excalidraw/nanoid` | `^5.1.11` | Retain the reviewed security/compatibility resolution for this dependency family; do not replace all parent majors with a single version. |
| `**/engine.io-client/ws` | `^8.22.0` | Retain the reviewed security/compatibility resolution for this dependency family; do not replace all parent majors with a single version. |
| `**/filelist/minimatch` | `^5.1.9` | Retain the reviewed security/compatibility resolution for this dependency family; do not replace all parent majors with a single version. |
| `@grpc/grpc-js` | `^1.14.6` | Retain the reviewed security/compatibility resolution for this dependency family; do not replace all parent majors with a single version. |
| `**/jake/minimatch` | `^3.1.4` | Retain the reviewed security/compatibility resolution for this dependency family; do not replace all parent majors with a single version. |
| `vite` | `^6.4.4` | Retain the reviewed security/compatibility resolution for this dependency family; do not replace all parent majors with a single version. |
| `**/anymatch/picomatch` | `^2.3.2` | Retain the reviewed security/compatibility resolution for this dependency family; do not replace all parent majors with a single version. |
| `**/micromatch/picomatch` | `^2.3.2` | Retain the reviewed security/compatibility resolution for this dependency family; do not replace all parent majors with a single version. |

For every entry above, remove or narrow it only after the parent dependency naturally resolves a patched compatible version, a fresh lockfile proves the vulnerable path is absent, the affected tests pass, and the relevant application/docs/MCP build passes. Change one dependency family at a time. Preserve parent-specific major-version constraints. Do not use audit suppression or a fabricated package version as a substitute for a fix.

## Known limitations at this baseline

The application audit retains a moderate advisory keyed to the workspace package version 0.18.0 even though related security components were updated. The separate unmaintained dev-docs lock has additional advisories, including critical findings; it needs its own migration before use. These findings are not all dependencies of the deployed drawing application.

## Future upstream sync and release

1. Compare upstream changes against the preserved paths above and keep unrelated local work intact.
2. Use the repository package manager and lockfile; review scoped overrides and affected regression tests.
3. Keep source checks, CI, provider build readiness, live-page checks, and external-service behavior as separate evidence.
4. Inspect added files and diffs for credentials before publishing. Store real credentials, deployment addresses for private services, user data and environment values outside Git.
5. After the reviewed commit is deployed, verify the intended domain and indexing policy. Remove only task-created branches that are already merged; preserve unrelated and unmerged branches.
