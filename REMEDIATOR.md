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

## Checks surface (Validate #1 / #2)

Step 4 is painted by [`.github/workflows/remediator-validate.yml`](.github/workflows/remediator-validate.yml) ("Remediator validate"), which runs on every pull request and on pushes to `master`. Its two jobs are the red → green surface: **Validate #1** re-scans with Semgrep using the pinned CWE-89 rules and fails while that class still matches at the Finding location (`routes/login.ts`), and **Validate #2** runs the plant's existing `test:frontend` / `test:server` / `test:api` scripts. The Semgrep gate is deliberately scoped to the Finding location rather than the whole repo, because the plant keeps other intentional SQL injection vulns (for example `routes/search.ts`) that a remediating PR is not supposed to touch — a repo-wide gate would stay red forever and there would be no green to land on. Upstream `ci.yml` is left alone; it gates most of its matrix on `github.repository == 'juice-shop/juice-shop'`, so it cannot be relied on for Checks in this fork. Upstream's `pr-compliance.yml` is gated the same way, because it runs on `pull_request_target` and auto-closes PRs it scores as spam — in this fork it scored the remediation PRs as spam for targeting `master` and for describing their own Semgrep re-scan. Fork-wide prerequisite: GitHub Actions must stay **enabled** in repository settings, or no Checks appear at all.

## Scope

Keep **intentional demo vulns** only. Do **not**:

- add exploits
- run scanners against production
- add new vulnerable endpoints
