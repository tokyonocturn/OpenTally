# Security policy

## Supported versions

| Version | Supported |
|---|---|
| 1.0.x | Yes |
| Earlier versions | No |

Security fixes are made against the latest 1.0.x release. Older copies should be upgraded before reporting an
issue that may already be fixed.

## Reporting a vulnerability

**Do not open a public GitHub issue for a security vulnerability.** Public reports can put users at risk before
a fix is available.

Use GitHub's private vulnerability reporting for this repository:

1. Open the repository's **Security** tab.
2. Choose **Report a vulnerability**.
3. Include the affected version, browser and operating system, a minimal reproduction, and the potential impact.

If private reporting is unavailable, contact a project maintainer privately through GitHub and ask for a secure
channel. Do not include exploit details in a public issue, discussion, or pull request.

The maintainers aim to acknowledge reports and provide an initial assessment within seven days. Please allow
reasonable time for investigation and a coordinated fix before public disclosure.

## Scope

OpenTally is a static, client-side application. Relevant issues include, but are not limited to:

- Cross-site scripting or unsafe rendering of labels, notes, category names, or imported data
- Validation bypasses in JSON import that can corrupt or expose local data
- CSV injection or unsafe spreadsheet formula handling
- Service-worker cache poisoning, unexpected cross-origin requests, or data exposure through the offline shell
- Dependency vulnerabilities that are exploitable in this application
- Accidental transmission of ledger data to a third party

The following are generally outside this policy:

- Attacks that require physical access to an unlocked device
- A compromised browser, browser extension, operating system, or hosting account
- Findings that require the user to paste malicious code into their own developer console
- Missing security headers on a deployment, unless they directly enable an application vulnerability
- Dependency version alerts without a demonstrated exploit path in OpenTally

## Handling of user data

OpenTally stores user data locally in IndexedDB and never sends ledger contents to a server. When reporting an
issue, use synthetic data and redact personal amounts, labels, notes, and backup files. Never attach a real
financial export to a public issue.

## Disclosure

After a fix is available, the maintainers may publish a security advisory describing the affected versions, the
impact, the fix, and any mitigation. Credit can be given to the reporter if they request it and disclosure is
coordinated.

There is no bug bounty program.
