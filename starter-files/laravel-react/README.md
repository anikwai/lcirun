# Laravel React Starter Kit Profile

Use this profile for applications created from [`laravel/react-starter-kit`](https://github.com/laravel/react-starter-kit).

The official React starter kit already includes:

- Laravel Pint.
- Larastan and a project-specific `phpstan.neon`.
- npm scripts for ESLint, Prettier, TypeScript, and Vite.
- Composer scripts for local quality checks.

Copy only the tailored CI runner:

```bash
mkdir -p bin
cp starter-files/laravel-react/bin/ci bin/ci
chmod +x bin/ci
```

Do not copy the generic `phpstan.neon.dist` over the starter kit's existing `phpstan.neon`.

## Required Locks

Local signoff should be based on locked dependencies. A consuming project should commit:

- `composer.lock`
- `package-lock.json`

The upstream starter-kit skeleton may not include those lockfiles before project creation, but a real application should have them after setup.

## Checks

The tailored runner installs locked PHP and npm dependencies, runs strict Composer and npm security audits, builds frontend assets, and then delegates to the starter kit's `composer ci:check` script.

## Optional Husky Hooks

Use Husky when you want fast local guardrails before commits and pushes:

```bash
npm install --save-dev husky
npm pkg set scripts.prepare="husky"
npm run prepare
cp starter-files/laravel-react/husky/pre-commit .husky/pre-commit
cp starter-files/laravel-react/husky/pre-push .husky/pre-push
```

The `pre-commit` hook runs fast formatting, frontend lint, frontend type, and PHP style/static checks through the starter kit's existing scripts. The `pre-push` hook runs `./bin/ci`.

Husky does not replace local CI or GitHub signoff. Treat it as early feedback only.

## Signoff

After `./bin/ci` passes on a committed and pushed branch:

```bash
gh signoff lint static tests security
```
