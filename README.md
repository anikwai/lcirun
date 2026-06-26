# Local CI Signoff Scaffold

This repository is a copyable scaffold for Laravel projects that want local CI to be the authoritative quality gate and GitHub to record explicit signoff statuses.

## What To Copy

Copy these starter files into a Laravel project:

- `bin/ci`
- `phpstan.neon.dist`
- `LOCAL_CI.md`
- `starter-files/github-workflows/ci.yml` if you want the optional remote backup workflow

Then install the required project tools and GitHub signoff extension:

```bash
composer require --dev laravel/pint "larastan/larastan:^3.0" brianium/paratest
gh extension install basecamp/gh-signoff
```

Run the local checks:

```bash
./bin/ci
```

After the checks pass on a committed and pushed branch, publish signoff statuses:

```bash
gh signoff lint static tests security
```

See [LOCAL_CI.md](./LOCAL_CI.md) for the full adoption guide.
