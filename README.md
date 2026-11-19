# spendwise-transaction-service

The ledger: incomes, expenses and categories (transaction_db). Publishes TransactionCreated/Reversed events through a transactional outbox and reverses a transaction when budget-service rejects it (saga).

| | |
|---|---|
| Port | 8083 |
| Configuration | `transaction-service*.yml` in [spendwise-config](https://github.com/PrathameshMax/spendwise-config) |
| Shared code | `com.spendwise:spendwise-parent / spendwise-common / spendwise-events` from [spendwise-shared](https://github.com/PrathameshMax/spendwise-shared) |
| Image | `ghcr.io/prathameshmax/spendwise-transaction-service:<git-sha>` and `:main` |

## Build

Reading the shared packages needs a classic GitHub token with `read:packages` in
`GITHUB_PACKAGES_TOKEN` and a `<server>` with id `github` in `~/.m2/settings.xml`.

```bash
./mvnw -B verify
docker build --secret id=gh_token,env=GITHUB_PACKAGES_TOKEN -t spendwise-transaction-service .
```

## CI

`.github/workflows/ci.yml`: every pull request builds, tests and builds the image; every push to
`main` also pushes the image to GHCR, tagged with the commit SHA and `main`. The repository secret
`PACKAGES_READ_TOKEN` (classic token, `read:packages`) lets CI read the shared packages.
