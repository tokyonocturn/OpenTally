# Changelog

All notable changes to OpenTally are documented in this file.

The format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/), and the project uses
[Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

No changes yet.

## [1.0.0] - 2026-10-30

The first public release of OpenTally: a local-first calculator and labeled personal ledger.

### Added

- Full calculator with addition, subtraction, multiplication, division, parentheses, unary signs, decimals,
  percentages, clear, backspace, keyboard entry, and result copying.
- Exact decimal monetary arithmetic backed by `decimal.js`.
- Persistent calculation history with expression and result.
- Labeled transactions with amount, description, category, date and time, notes, and source calculation.
- Add, edit, duplicate, delete, search, filter, and sort transaction flows with confirmation before deletion.
- Derived running balances and a summary of opening balance, income, expenses, net change, and transaction count.
- Multiple independent tallies, each with its own opening balance, entries, categories, history, currency
  override, precision override, and archive state.
- Custom tally categories with default suggestions, colors, renaming, creation, and deletion.
- Local SQLite database running in the browser through `sql.js` (SQLite compiled to WebAssembly).
- Automatic persistence of database bytes to IndexedDB after every mutation.
- In-session undo and redo with a 40-step limit.
- Manual local snapshots plus automatic daily snapshots with seven-snapshot retention.
- Versioned JSON backup and restore for all data or a single tally, with complete validation before writes and
  merge or replace import modes.
- Spreadsheet-ready CSV export for all data or a single tally, including numeric amount and balance columns,
  UTF-8 BOM, and spreadsheet-formula neutralization.
- Light, dark, and system theme preferences.
- Configurable ISO 4217 currency, 0–6 decimal precision, number format, and date format.
- Responsive layout down to 320px, semantic controls, visible focus states, accessible dialogs, and
  reduced-motion support.
- Keyboard shortcuts for undo, redo, search, and opening a new entry.

### Branding and distribution

- Official OpenTally logo and brand system, including a transparent lockup, simplified mark, wordmark, and
  social card.
- Complete favicon, app icon, Apple touch icon, Android Chrome, and maskable icon package.
- Web app manifest, Open Graph and Twitter metadata, and an offline application-shell service worker for
  supported browsers.
- Prepared Windows, macOS, and Linux icon sources for a future native wrapper. No native desktop package,
  executable, tray, or Start Menu integration is included in 1.0.0.
- Professional README with genuine application screenshots, feature documentation, setup instructions, and
  privacy details.
- Contributor Covenant code of conduct, security policy, contributing guide, dependency inventory, editor
  configuration, issue forms, pull request template, and GitHub Actions CI.

### Security and privacy

- No accounts, telemetry, analytics, remote fonts, advertising, third-party runtime scripts, or paid APIs.
- No ledger data sent to a server; storage is local to the browser profile and origin.
- Strict validation for imported JSON, parameterized SQL, and decimal-string persistence for money.
- Dependency audit of the production tree completed with zero reported vulnerabilities at release time.

