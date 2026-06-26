# Local CI is authoritative

This scaffold treats local CI as the authoritative quality gate for Laravel projects, with GitHub recording developer-published signoff statuses before merge. A minimal GitHub Actions workflow may still exist as a backup, but branch protection is expected to enforce `gh-signoff` statuses rather than making remote CI the primary source of truth.
