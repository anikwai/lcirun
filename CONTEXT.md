# Local CI Signoff Scaffold

A reusable scaffold for teams that want Laravel projects to run CI locally and publish explicit GitHub signoff statuses before merge.

## Language

**Local CI Signoff Scaffold**:
A copyable set of project files that standardizes local Laravel checks and GitHub signoff statuses.
_Avoid_: Laravel app, CI app, build server

**Starter File**:
A file in the scaffold that a Laravel project can copy and adapt.
_Avoid_: generated file, installed file

**Signoff Status**:
A GitHub commit status that records a developer's attestation that a local check passed.
_Avoid_: CI job, workflow run

**Lint Signoff**:
A **Signoff Status** attesting that code style checks passed locally.
_Avoid_: formatting job

**Static Signoff**:
A **Signoff Status** attesting that static analysis passed locally.
_Avoid_: typecheck job

**Tests Signoff**:
A **Signoff Status** attesting that automated tests passed locally.
_Avoid_: test workflow

**Security Signoff**:
A **Signoff Status** attesting that dependency security checks were reviewed locally.
_Avoid_: audit job

**Strict Security Check**:
A dependency security check that must pass before **Security Signoff** is published.
_Avoid_: advisory audit

**Project Test Environment**:
The consuming Laravel project's own test configuration used by local CI.
_Avoid_: scaffold test database

**Dependency Freshness**:
The expectation that local CI installs locked project dependencies before checks run.
_Avoid_: bootstrap step

**Frontend Lockfile Detection**:
The scaffold rule that selects the frontend package manager from the consuming project's lockfile.
_Avoid_: npm-only frontend check

**Adoption Guide**:
The detailed documentation that explains how a Laravel project copies and uses the scaffold.
_Avoid_: Git guide, contribution policy

**Integration Profile**:
A tailored set of **Starter Files** for a known Laravel project shape.
_Avoid_: fork, preset, app template

## Relationships

- A **Local CI Signoff Scaffold** is copied into a Laravel project.
- A **Local CI Signoff Scaffold** contains one or more **Starter Files**.
- An **Integration Profile** adapts **Starter Files** to a known Laravel project shape.
- The **Adoption Guide** explains how a Laravel project uses the **Starter Files**.
- A Laravel project runs local checks before publishing **Signoff Statuses**.
- The canonical **Signoff Statuses** are **Lint Signoff**, **Static Signoff**, **Tests Signoff**, and **Security Signoff**.
- A **Security Signoff** requires a **Strict Security Check**.
- A **Tests Signoff** uses the **Project Test Environment** by default.
- Published **Signoff Statuses** assume **Dependency Freshness**.
- Frontend checks use **Frontend Lockfile Detection** when a consuming Laravel project has frontend assets.

## Example dialogue

> **Dev:** "Is this repo itself a Laravel project?"
> **Domain expert:** "No — it is a **Local CI Signoff Scaffold** that Laravel projects can adopt."
> **Dev:** "Should we only document the commands?"
> **Domain expert:** "No — include **Starter Files** so teams can copy the working defaults directly."
> **Dev:** "Does GitHub run the authoritative CI?"
> **Domain expert:** "No — GitHub records **Signoff Statuses** after the developer runs local CI."
> **Dev:** "Should signoff be one big check?"
> **Domain expert:** "No — publish separate **Lint Signoff**, **Static Signoff**, **Tests Signoff**, and **Security Signoff** statuses."
> **Dev:** "Can the security audit fail while still publishing **Security Signoff**?"
> **Domain expert:** "No — **Security Signoff** requires a **Strict Security Check**."
> **Dev:** "Should the scaffold force SQLite in-memory tests?"
> **Domain expert:** "No — **Tests Signoff** should use the **Project Test Environment** unless a project opts into a faster preset."
> **Dev:** "Can I skip dependency installation before local CI?"
> **Domain expert:** "No — published **Signoff Statuses** assume **Dependency Freshness**."
> **Dev:** "Does every consuming project have to use npm?"
> **Domain expert:** "No — frontend checks use **Frontend Lockfile Detection**."
> **Dev:** "Should this live in `CONTRIBUTING.md`?"
> **Domain expert:** "No — the scaffold should have an **Adoption Guide** that can be copied without replacing a project's contribution policy."
> **Dev:** "Should a `laravel/react-starter-kit` app use the generic scaffold unchanged?"
> **Domain expert:** "No — use an **Integration Profile** that wraps the starter kit's existing scripts."

## Flagged ambiguities

- "repo" could mean either the scaffold repository or a consuming Laravel project — resolved: this repository is the **Local CI Signoff Scaffold**.
