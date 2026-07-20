# Local CI signoff scaffold

This repository is a copyable scaffold for Laravel projects (but can be used in any stack) that want local CI to be the authoritative quality gate and GitHub to record explicit signoff statuses. No need to use this if your codebase is small.

<img width="1632" height="798" alt="image" src="https://github.com/user-attachments/assets/8e16f81c-794e-41e2-855a-4589140d7f6b" />


## What To Copy

Copy these starter files into a Laravel project:

- `bin/ci`
- `phpstan.neon.dist`
- `LOCAL_CI.md`
- `starter-files/github-workflows/ci.yml` if you want the optional remote backup workflow

For applications created from `laravel/react-starter-kit`, use the tailored profile instead:

- `starter-files/laravel-react/bin/ci`
- `starter-files/laravel-react/README.md`

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


