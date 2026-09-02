# Vuln Remediator (this fork)

This repository is the **Path B plant** for Wood's Vuln Remediator: a Juice Shop checkout that Cloud Agents open remediating PRs against. It is not a general-purpose Juice Shop fork.

## Runtime (LOCKED)

The app runs only on:

- the **presenter localhost**, or
- a **Cloud Agent VM** bound to `127.0.0.1` via Environments

**Never** Internalsphere, Vercel, or any public HTTP surface.

## Harness vs this repo

Harness and docs live in **`internalsphere/wood-vuln-remediation`**. That repo does **not** run Juice Shop. This repo is the plant only.

## Demo spine

1. **Finding** — a known, intentional vuln in this plant.
2. **Four-field Job** — operator submits the remediator job.
3. **Cloud Agent PR** — the agent opens a PR **against this repo**.
4. **Checks** — Semgrep re-scan plus existing `npm test` / merge tests on that PR.
5. **Ta-da** — Checks go **red → green**, plus Cloud Agent artifacts.

The advanced UI clip is **jumpable** and is **never a gate**.

## Scope

Keep **intentional demo vulns** only. Do **not**:

- add exploits
- run scanners against production
- add new vulnerable endpoints
