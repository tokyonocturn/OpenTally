<p align="center">
  <img src="assets/branding/opentally-logo.png" alt="OpenTally logo" width="760" />
</p>

<h1 align="center">OpenTally</h1>

<p align="center"><strong>Calculate. Label. Keep Track.</strong></p>

<p align="center">
  An open-source calculator and personal ledger for people who want to know what every amount represents.
  Do the arithmetic, attach a label, and keep a running balance across independent tallies.
</p>

<p align="center">
  <img alt="Version 1.0.0" src="https://img.shields.io/badge/version-1.0.0-7c55f0" />
  <img alt="96 tests passing" src="https://img.shields.io/badge/tests-96%20passing-brightgreen" />
  <img alt="MIT License" src="https://img.shields.io/badge/license-MIT-blue" />
</p>

## About OpenTally

OpenTally combines a calculator with a labeled transaction ledger. A normal calculator gives you a result and forgets why you calculated it. OpenTally keeps the amount, label, category, date, and the expression that produced it, then shows how each entry changes the running balance.

```text
Starting balance              $2,000.00
-600.00   Amazon              $1,400.00
-400.00   Bullets             $1,000.00
+1,500.00 Paycheck            $2,500.00
-85.00    Groceries           $2,415.00
-120.00   Gas                 $2,295.00
+200.00   Refund              $2,495.00
```

Use one tally for everyday spending, another for a savings goal, another for a trip, and another for a project. Each tally has its own opening balance, entries, categories, currency, precision, and history. There is no account to create, no financial service to connect, and no server storing your data.

## Screenshots

These are captures of the running application with synthetic demo data, not mockups.

| Main calculator | Transaction history |
|:---:|:---:|
| [![Main calculator interface](assets/screenshots/main-calculator.png)](assets/screenshots/main-calculator.png) | [![Labeled transaction history with running balances](assets/screenshots/transactions.png)](assets/screenshots/transactions.png) |
| Calculate, label, and add an entry | Search, filter, and inspect labeled transactions |

| Multiple tallies | Summary and running balance |
|:---:|:---:|
| [![Multiple independent tallies](assets/screenshots/multiple-tallies.png)](assets/screenshots/multiple-tallies.png) | [![Summary panel with income, expenses, net change, and running balance](assets/screenshots/summary-running-balance.png)](assets/screenshots/summary-running-balance.png) |
| Independent balances for separate parts of life | Opening balance, income, expenses, net change, and current balance |

| Light theme | Dark theme |
|:---:|:---:|
| [![OpenTally in light mode](assets/screenshots/light-mode.png)](assets/screenshots/light-mode.png) | [![OpenTally in dark mode](assets/screenshots/dark-mode.png)](assets/screenshots/dark-mode.png) |
| Warm paper-like light theme | Low-glare dark theme |

| Import and export | Mobile layout |
|:---:|:---:|
| [![JSON backup, CSV export, and local backup dialog](assets/screenshots/import-export.png)](assets/screenshots/import-export.png) | [![OpenTally on a narrow mobile screen](assets/screenshots/mobile.png)](assets/screenshots/mobile.png) |
| JSON restores, CSV exports, and local snapshots | Responsive down to a 320px-wide viewport |

## Features

### Calculator

- Addition, subtraction, multiplication, and division with parentheses, unary signs, decimals, and correct order of operations.
- Calculator-style percentages: `100 + 10%` is `110`, `100 - 10%` is `90`, and `200 * 10%` is `20`.
- Exact monetary arithmetic with `decimal.js`; `0.10 + 0.20` is exactly `0.30`.
- Division-by-zero protection, malformed-expression handling, clear, and backspace.
- Keyboard entry, `Enter` to evaluate, `Escape` to clear, and one-click result copying.
- Persistent calculation history with the expression that produced each result.

### Labeled ledger

- Every transaction stores an amount, label, category, date and time, optional notes, and its source calculation when applicable.
- Add, edit, duplicate, search, filter, sort, and delete entries. Deletions require confirmation by default.
- A derived running balance on every row, plus opening balance, income, expenses, net change, and transaction count.
- Custom categories per tally, with names and colors. Default categories are suggestions, not locked choices.

### Multiple tallies

- Keep any number of independent tallies, each with its own opening balance, entries, categories, history, currency override, and precision override.
- Switch tallies from the desktop sidebar or the mobile selector.
- Archive a tally without deleting its data. OpenTally always keeps at least one tally.

### Search and organization

- Search labels, notes, categories, amounts, dates, and calculations.
- Filter by income or expense, category, and date range.
- Sort oldest first, newest first, highest amount, or lowest amount.

### Data and portability

- Local SQLite database running in the browser through WebAssembly, saved to IndexedDB after every change.
- Manual local snapshots plus automatic daily snapshots, keeping the seven newest.
- Versioned JSON backup and restore for all data or one tally, with validation before any write.
- Spreadsheet-ready CSV export with numeric amount and balance columns.
- Undo and redo for up to 40 in-session changes.
- Installable PWA metadata and an offline application shell after the first successful visit.

### Interface

- Light, dark, and system theme preferences.
- Configurable ISO 4217 currency, 0 to 6 decimal places, number format, and date format.
- Keyboard shortcuts for undo, redo, search, and new entries.
- Responsive layout from desktop down to narrow phones.
- Semantic controls, visible focus states, accessible dialogs, and reduced-motion support.
- No accounts, advertisements, analytics, tracking, remote fonts, or third-party runtime scripts.

## Technology stack

| Technology | Purpose |
|---|---|
| React 19 and TypeScript 5.9 | UI components and strict type checking |
| Vite 8 | Development server, fast refresh, and production bundling |
| sql.js 1.15 | SQLite compiled to WebAssembly, running in the browser |
| decimal.js 10.6 | Arbitrary-precision decimal arithmetic for money |
| IndexedDB | Durable storage for the SQLite database and local snapshots |
| Vitest 5 and Testing Library | Unit, integration, and UI tests |
| Web App Manifest and Service Worker | Installable PWA metadata and offline shell |
| CSS custom properties | Themes, layout, and responsive design without a UI framework |

### Persistence architecture

The UI renders from a single `OpenTallyApp` service. React state is never the source of truth.

```text
UI action
  -> validated OpenTallyApp mutation
    -> SQLite transaction
      -> database bytes exported and saved to IndexedDB
        -> fresh application state queried from SQLite
          -> React rerenders
```

- The schema is defined in `src/db/schema.ts` and versioned with `PRAGMA user_version`.
- Tables: `tallies`, `categories`, `entries`, `settings`, and `calculations`.
- Foreign keys are enabled. Deleting a tally cascades to its categories and entries. Deleting a category sets its entries' category to `NULL` and keeps the entries.
- Amounts are stored as fixed-point decimal strings, never IEEE-754 numbers.
- Running balances are derived from the ledger, not stored in a mutable balance field.
- JSON import is validated completely before a transaction begins, so a malformed file cannot partially modify existing data.

## Quick start

### Requirements

- [Node.js](https://nodejs.org/) 20.19 or newer. Node.js 22 or 24 LTS is recommended.
- npm, included with Node.js.
- A current browser with WebAssembly and IndexedDB support.

No API keys, `.env` file, database server, or account are required.

Clone or download the repository, then run:

```bash
npm ci
npm run dev
```

Open the URL printed by Vite, normally <http://localhost:5173>. The first launch creates a local database with a default **Personal Expenses** tally.

## Development

| Command | What it does |
|---|---|
| `npm run dev` | Start the Vite development server with hot reload |
| `npm test` | Run the complete Vitest suite once |
| `npm run test:watch` | Run tests in watch mode |
| `npm run typecheck` | Run TypeScript without emitting files |
| `npm run build` | Type-check, then build the production site into `dist/` |
| `npm run preview` | Serve the production build locally |

Run the full check before opening a pull request:

```bash
npm test
npm run typecheck
npm run build
```

The suite contains 96 tests across 11 test files. It covers the expression engine, decimal money handling, quick-entry parsing, ledger calculations, database repositories, migrations, persistence, undo and redo, categories, tallies, backups, import and export, and the main UI flows.

### Project structure

```text
.
├── .github/                   CI workflow, issue forms, and pull request template
├── assets/
│   ├── branding/              Official logo, icon sources, social card, desktop sources
│   └── screenshots/           Application screenshots used by this README
├── public/
│   ├── icons/                 Favicons, app icons, and maskable icon
│   ├── manifest.webmanifest   PWA metadata
│   ├── og-image.png           Open Graph and social preview
│   └── sw.js                  Offline application-shell service worker
├── scripts/                   Reproducible brand asset generation
├── src/
│   ├── lib/                   Money, expression, quick-entry, dates, CSV, downloads
│   ├── domain/                Types, ledger calculations, settings, and transfer validation
│   ├── db/                    SQLite schema, wrapper, and repositories
│   ├── runtime/               Application service, IndexedDB, and bootstrap
│   └── ui/                    Panels, dialogs, components, icons, and theme hook
├── tests/                     Vitest unit, integration, persistence, and UI tests
├── index.html
├── package.json
├── package-lock.json
├── tsconfig.json
└── vite.config.ts
```

## Building

```bash
npm run build
npm run preview
```

The static site is written to `dist/`. It can be hosted on any static file server.

- Serve JavaScript with a JavaScript MIME type and `.wasm` files as `application/wasm`. The SQLite engine will not initialize with the wrong MIME type.
- No rewrite rules or server-side runtime are required.
- The build uses relative asset URLs, so it works under a project path such as GitHub Pages.
- Service workers and PWA installation require HTTPS, except on localhost during development.

The included CI workflow tests, type-checks, and builds the project. It does not deploy it.

## Data and privacy

OpenTally is designed to work without an account, a backend, or a paid API.

- **Local by default.** Financial data is stored in the browser's IndexedDB as SQLite database bytes. The application does not send entries, balances, settings, or calculations to a server.
- **No telemetry.** There is no analytics, crash reporting, advertising, tracking pixel, remote font, or third-party runtime script.
- **Origin-scoped.** Data belongs to one browser profile and origin. Local development, a production domain, a private window, and another browser each have separate storage.
- **Backups are local.** Manual and automatic snapshots live in the same IndexedDB database. They do not survive clearing site data. Download a JSON backup to a separate location for disaster recovery.
- **Unencrypted at rest.** The application does not encrypt browser storage. Use full-disk encryption and a secure OS account if the device is shared.
- **Export is explicit.** JSON and CSV downloads happen only when you choose them. Clipboard access is user-initiated.
- **The service worker** caches only the application shell: HTML, JavaScript, CSS, WebAssembly, icons, and the manifest. It does not upload or copy financial data.

A static host can see ordinary request metadata such as an IP address and requested asset, as with any website. It never receives ledger contents.

## Import and export

Open **Data** in the header.

- **JSON, all data** preserves tallies, categories, entries, preferences, opening balances, and up to 50 recent calculations.
- **JSON, this tally** preserves only that tally's categories and entries. Global preferences and calculation history are omitted.
- **CSV, all data** and **CSV, this tally** export date, time, label, category, amount, running balance, notes, and calculation. Numeric columns are unformatted decimal strings so spreadsheets can total them.
- **Import** accepts OpenTally version 1 JSON. Files are validated before any write. Choose **Add to current data** to create new IDs alongside existing records, or **Replace all data** to restore the backup exactly.
- **Local backups** can be created, restored, and deleted in the same panel. **Erase all data** removes all tallies, entries, categories, preferences, and local snapshots after confirmation.

CSV is export-only. Use JSON to restore data. CSV output includes a UTF-8 BOM for Excel compatibility and neutralizes text cells that begin with spreadsheet formula characters.

## Keyboard shortcuts

| Shortcut | Action |
|---|---|
| `Ctrl` or `Command` + `Z` | Undo the last change |
| `Ctrl` or `Command` + `Shift` + `Z`, or `Ctrl` or `Command` + `Y` | Redo |
| `/` | Focus ledger search |
| `Alt` + `N` | Open a new entry |
| `Enter` | Evaluate the calculator or submit a dialog |
| `Escape` | Close a dialog or clear the calculator |

## Native desktop packaging

OpenTally 1.0.0 is a browser and PWA application. It does not ship a Windows executable, macOS app bundle, Linux package, system tray, or Start Menu shortcut.

The official icon sources for a future desktop wrapper are included in `assets/branding/desktop/`:

- `opentally-windows.ico` for a Windows executable, window, and shortcut
- `opentally-macos-1024.png` as the source for a macOS `.icns` bundle icon
- `opentally-linux-512.png` as the source for a Freedesktop `.desktop` icon

There is no current Electron or Tauri configuration.

## Limitations

- No bank connections, cloud sync, multi-device sync, multi-user access, or currency conversion.
- Data is local to one browser profile and origin, and is not encrypted by the application.
- CSV is export-only.
- Undo history is kept in memory for the current session.
- Automatic backups are stored in the same browser, so they are not a substitute for an external JSON backup.
- PWA installation and offline caching depend on browser support. The first visit must succeed before the shell is available offline.

## Contributing

Contributions are welcome. Read [CONTRIBUTING.md](CONTRIBUTING.md) for setup, testing, code, and privacy expectations. By participating, you agree to follow [CODE_OF_CONDUCT.md](CODE_OF_CONDUCT.md).

- [Report a bug](.github/ISSUE_TEMPLATE/bug_report.yml)
- [Request a feature](.github/ISSUE_TEMPLATE/feature_request.yml)
- [Read the security policy](SECURITY.md)

Never attach real financial data, exported backups, or screenshots containing personal amounts to an issue or pull request. Use synthetic examples.

## License

OpenTally is released under the [MIT License](LICENSE).

Third-party packages and their licenses are documented in [DEPENDENCIES.md](DEPENDENCIES.md). The app contains no proprietary SDKs, paid APIs, or telemetry.
