# Contributing to OpenTally

Thanks for helping improve OpenTally. This project aims to stay small, dependable and local-first: a calculator
with labels and a running balance, not a full accounting suite.

## Before you start

- Search existing issues before opening a new one.
- Keep changes focused. Small pull requests are easier to review and easier to undo.
- For a substantial feature or a schema change, open an issue first so the approach can be discussed.
- Never attach real financial data, exported backups, or screenshots containing personal amounts to an issue or
  pull request. Create synthetic fixtures instead.

## Set up the project

Requirements: Node.js 20.19 or newer and npm.

```bash
npm ci
npm run dev
```

The app runs entirely in the browser. There is no backend, account or `.env` file to configure.

## Checks

Run these before opening a pull request:

```bash
npm test
npm run typecheck
npm run build
```

Please run the production build as well as the tests. A successful development reload does not prove the production
bundle (including the sql.js WebAssembly asset) is valid.

The suite is in `tests/`:

| File | Focus |
|---|---|
| `money.test.ts` | Decimal parsing, precision, formatting |
| `expression.test.ts` | Calculator grammar, precedence, percentages, errors |
| `quickEntry.test.ts` | Amount-first and label-first shorthand |
| `toggleSign.test.ts` | Calculator sign toggling |
| `csv.test.ts` | Escaping, numeric cells and spreadsheet-injection protection |
| `ledger.test.ts` | Running balances, summaries, filtering and sorting |
| `database.test.ts` | Schema, repository and migration behavior |
| `app.test.ts` | Mutations, persistence, undo/redo, tallies, backups, import/export |
| `transfer.test.ts` | Versioned JSON validation and round trips |
| `indexedDb.test.ts` | IndexedDB byte and snapshot stores |
| `ui.test.tsx` | Core calculator, quick-entry and entry-dialog flows |

Add a regression test for every bug fix. For a financial calculation change, test both the normal case and a
boundary case (negative values, zero, precision rounding, or a very large amount as appropriate).

## Code guidelines

### Money

- Use `decimal.js` through [src/lib/money.ts](src/lib/money.ts) for all monetary arithmetic.
- Never use binary floating-point arithmetic (`+`, `-`, `*`, `/` on JavaScript `number`) to calculate balances or
  store amounts.
- Persist amounts as fixed-point decimal strings. Do not store them as `number`.
- Round using the existing helpers and the configured precision. Avoid ad hoc `toFixed` calls in the UI.
- Recalculate derived balances from the ledger rather than storing a mutable balance field.

### Data and persistence

- All writes go through `OpenTallyApp`. Do not write to IndexedDB directly from a component.
- Validate input at the runtime/domain boundary, including imported files. Reject invalid data before opening a
  write transaction; a malformed import must never partially modify existing records.
- Wrap multi-row writes in a SQLite transaction.
- If you change the SQLite schema, increment `SCHEMA_VERSION` and add a forward migration. Do not edit an already
  released version in place. Add a test that opens a database from the previous schema version.
- Preserve the database and JSON import-format compatibility. A JSON schema change requires a new version and
  explicit import handling.
- Use parameterized SQL. Never interpolate user text into SQL.

### UI

- Follow the existing design tokens and component styles in [src/styles.css](src/styles.css).
- Keep the interface responsive down to narrow phones and usable at desktop widths.
- Use semantic HTML, visible focus states, labels for every form control, and accessible dialogs. Do not rely on
  color alone to convey meaning.
- Use the existing modal, category picker and toast components rather than creating parallel patterns.
- Keep animations minimal and respect the project's restrained visual style.
- Add or update a UI test for a user-visible flow when practical.

### Privacy

OpenTally must remain fully local. Do not add:

- Analytics, telemetry, error reporting, remote logging or tracking
- Advertising, remote fonts, or third-party scripts
- Accounts, cloud sync, or a required backend
- Paid APIs or credentials
- Secret values or personal data in committed fixtures

Clipboard access must remain user-initiated. Downloads must remain explicit user actions.

## Community and security

By participating in this project, you agree to follow the [Code of Conduct](CODE_OF_CONDUCT.md). Please be
respectful in issues, pull requests, and discussions.

Do not report security vulnerabilities in a public issue. Follow [SECURITY.md](SECURITY.md) and use the
repository's private vulnerability reporting instead.

## Pull requests

1. Fork the repository and create a focused branch.
2. Include tests for behavior changes and update the README if the user-facing behavior changes.
3. Run `npm test`, `npm run typecheck` and `npm run build`.
4. Describe the problem, the approach and any user-visible tradeoffs in the pull request.
5. Keep unrelated formatting or refactoring out of the change.
6. Do not bump dependency versions or add packages without explaining why the change is necessary and confirming
   the license is compatible with MIT.

By contributing, you agree that your contribution is licensed under the project's [MIT License](LICENSE).
