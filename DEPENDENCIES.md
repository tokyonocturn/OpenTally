# Dependency inventory

OpenTally 1.0.0 uses only open-source packages. There are no proprietary SDKs, paid APIs, telemetry libraries,
remote fonts, or runtime services.

The direct dependencies are listed below. Exact resolved versions and integrity hashes for the full tree are
recorded in `package-lock.json`; install with `npm ci` to reproduce them.

## Runtime dependencies

| Package | Version | License | Why it is used |
|---|---:|---|---|
| [`decimal.js`](https://github.com/MikeMcl/decimal.js) | 10.6.0 | MIT | Arbitrary-precision decimal arithmetic for every monetary calculation |
| [`react`](https://react.dev/) | 19.3.1 | MIT | UI component runtime |
| [`react-dom`](https://react.dev/) | 19.3.1 | MIT | Rendering the UI into the browser DOM |
| [`sql.js`](https://github.com/sql-js/sql.js) | 1.15.2 | MIT | SQLite compiled to WebAssembly for the local relational database |

`sql.js` embeds SQLite, which is in the public domain. The WebAssembly build is shipped as a static asset in
`dist/assets/` after building.

## Development dependencies

| Package | Version | License | Why it is used |
|---|---:|---|---|
| [`@testing-library/jest-dom`](https://github.com/testing-library/jest-dom) | 7.0.1 | MIT | DOM assertions in UI tests |
| [`@testing-library/react`](https://github.com/testing-library/react-testing-library) | 16.3.3 | MIT | Rendering React components in tests |
| [`@testing-library/user-event`](https://github.com/testing-library/user-event) | 14.6.8 | MIT | Realistic user interaction in UI tests |
| [`@types/react`](https://github.com/DefinitelyTyped/DefinitelyTyped) | 19.3.0 | MIT | TypeScript definitions for React |
| [`@types/react-dom`](https://github.com/DefinitelyTyped/DefinitelyTyped) | 19.3.0 | MIT | TypeScript definitions for React DOM |
| [`@types/sql.js`](https://github.com/DefinitelyTyped/DefinitelyTyped) | 1.4.11 | MIT | TypeScript definitions for sql.js |
| [`@vitejs/plugin-react`](https://github.com/vitejs/vite-plugin-react) | 6.1.2 | MIT | React support in Vite |
| [`fake-indexeddb`](https://github.com/dumbmatter/fakeIndexedDB) | 6.2.7 | Apache-2.0 | In-memory IndexedDB implementation for tests |
| [`jsdom`](https://github.com/jsdom/jsdom) | 30.1.3 | MIT | DOM environment for UI tests |
| [`typescript`](https://github.com/microsoft/TypeScript) | 5.9.3 | Apache-2.0 | Type checking and compilation |
| [`vite`](https://github.com/vitejs/vite) | 8.3.5 | MIT | Development server and production bundling |
| [`vitest`](https://github.com/vitest-dev/vitest) | 5.0.5 | MIT | Test runner and assertions |

## Transitive dependency licenses

At release time, `package-lock.json` resolves 133 installed package entries (direct and transitive) under these
licenses:

| License | Package entries |
|---|---:|
| MIT | 105 |
| MPL-2.0 | 12 |
| Apache-2.0 | 6 |
| MIT-0 | 2 |
| BSD-2-Clause | 2 |
| BSD-3-Clause | 2 |
| ISC | 2 |
| BlueOak-1.0.0 | 1 |
| CC0-1.0 | 1 |

All listed licenses are open-source licenses or public-domain dedications. MPL-2.0 is file-level copyleft; those
packages are build-time tooling and do not change the license of OpenTally's own source. CC0-1.0 is a public-domain
dedication. There are no `UNLICENSED` or unknown entries in the locked tree.

## Reproducing the inventory

Show the resolved dependency graph:

```bash
npm ls --all
```

Print the license field recorded for every package in the installed lockfile:

```bash
node -e "const lock=require('./package-lock.json');for(const [k,v] of Object.entries(lock.packages)){if(k.startsWith('node_modules/'))console.log(k.replace('node_modules/','')+' | '+(v.version||'')+' | '+(v.license||'UNKNOWN'))}"
```

Review the full text of any package license in its `node_modules/<package>/LICENSE*` file.

If you add a dependency, update this inventory and confirm that its license is compatible with the project's
[MIT License](LICENSE). This document is an engineering inventory, not legal advice.
