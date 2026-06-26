# Local CI for Laravel Projects

Modern developer machines are fast enough to run linting, static analysis, security checks, and tests locally. This scaffold treats local CI as the authoritative quality gate, then records the result in GitHub through `gh-signoff` statuses.

The model is inspired by DHH/Basecamp's local-CI approach: run the checks where the code is written, then require explicit signoff statuses before merge.

## Why This Approach?

- Fast feedback without waiting for a remote queue.
- Lower CI maintenance for mid-sized Laravel applications.
- One local command that matches the team's merge contract.
- GitHub branch protection still enforces visible status checks.

## Starter Files

Copy these files into a Laravel project:

```text
bin/ci
phpstan.neon.dist
LOCAL_CI.md
starter-files/github-workflows/ci.yml
```

The workflow file is optional. Copy it to `.github/workflows/ci.yml` in the consuming project when the project still wants a remote backup check.

The starter defaults to PHP 8.5 for the optional GitHub Actions backup workflow.

## Install Required Tools

Install the Laravel quality tools in the consuming project:

```bash
composer require --dev laravel/pint "larastan/larastan:^3.0" brianium/paratest
```

Install the GitHub CLI extension:

```bash
gh extension install basecamp/gh-signoff
```

## Run Local CI

Run:

```bash
./bin/ci
```

The starter script runs:

- `composer install` for dependency freshness.
- `composer audit` as a strict security check.
- Laravel Pint in test mode.
- Larastan/PHPStan.
- Laravel tests using the project's own test environment.
- Frontend build when a recognized frontend lockfile exists.

The PHP dependency check expects `composer.lock` to be committed. If the project does not have a lockfile, create and commit one before relying on local signoff.

By default, tests respect the consuming project's `.env.testing` and test database configuration. SQLite in-memory tests can be a useful project-specific speed optimization, but the scaffold does not force SQLite because that can hide MySQL, PostgreSQL, collation, JSON, queue, tenancy, or migration behavior.

## Publish GitHub Signoff Statuses

`gh-signoff` signs the current commit SHA, so the commit must already be pushed unless you intentionally force signoff. After `./bin/ci` passes on a committed and pushed branch:

```bash
gh signoff lint static tests security
```

These publish the canonical signoff statuses:

- `signoff/lint`
- `signoff/static`
- `signoff/tests`
- `signoff/security`

You can also publish one status at a time:

```bash
gh signoff lint
gh signoff static
gh signoff tests
gh signoff security
```

## Install Branch Protection

For a `main` branch:

```bash
gh signoff install --branch main lint static tests security
```

Use `gh signoff install` as a new-repo convenience. On an existing protected branch, inspect the result afterward or add the contexts manually, because the extension updates branch protection through the GitHub API and may replace existing protection details such as review or restriction settings.

If you configure branch protection manually, require these status contexts:

```text
signoff/lint
signoff/static
signoff/tests
signoff/security
```

## Daily Workflow

1. Make changes.
2. Run `./bin/ci`.
3. Commit and push the branch.
4. Run `./bin/ci` again if anything changed after the previous run.
5. Run `gh signoff lint static tests security`.
6. Open or refresh the pull request.
7. Merge only when GitHub shows the required signoff statuses.

## Frontend Package Managers

The starter script detects the frontend package manager by lockfile:

```text
bun.lockb or bun.lock -> bun install --frozen-lockfile && bun run build
pnpm-lock.yaml       -> pnpm install --frozen-lockfile && pnpm run build
yarn.lock            -> yarn install --frozen-lockfile && yarn build
package-lock.json    -> npm ci --no-audit && npm run build
```

If a project has `package.json` but no recognized lockfile, or if it has multiple recognized frontend lockfiles, the script fails with a clear message instead of guessing.

## Optional Remote Backup

The included `starter-files/github-workflows/ci.yml` runs `./bin/ci` on GitHub Actions after it is copied to `.github/workflows/ci.yml` in a consuming project. Treat it as a backup or sanity check, not the primary source of truth, unless your team intentionally changes the contract.

## Customizations

- Add `npm run lint`, `pnpm test`, or frontend type checks when the project has them.
- Add Rector with `./vendor/bin/rector process --dry-run`.
- Raise PHPStan's level over time.
- Run `bin/ci` inside Sail or another container if the project requires container parity.
- Use SQLite in-memory tests only when the project has confirmed that this matches production-relevant behavior.

## Resources

- [gh-signoff](https://github.com/basecamp/gh-signoff)
- [Laravel Pint](https://laravel.com/docs/pint)
- [Larastan](https://github.com/larastan/larastan)
