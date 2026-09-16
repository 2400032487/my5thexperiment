# Git Flow Workflow

This repository uses a Git Flow-style branching model.

## Permanent branches

- `main`: production-ready code only.
- `develop`: integration branch for the next release.

## Branch naming

- `feature/<short-description>`: new work branched from `develop`.
- `release/<version>`: release stabilization branched from `develop`.
- `hotfix/<short-description>`: urgent production fix branched from `main`.

## Start future development

```bash
git switch develop
git pull origin develop
git switch -c feature/<short-description>
```

Commit focused changes, then publish the feature branch:

```bash
git add .
git commit -m "Add <short description>"
git push -u origin feature/<short-description>
```

Open a pull request from `feature/<short-description>` into `develop`. After it is reviewed and merged, delete the feature branch.

## Prepare a release

```bash
git switch develop
git pull origin develop
git switch -c release/<version>
```

Only stabilization changes belong on a release branch. Merge the release into both `main` and `develop`, then tag `main`:

```bash
git switch main
git merge --no-ff release/<version>
git tag -a <version> -m "Release <version>"

git switch develop
git merge --no-ff release/<version>

git branch -d release/<version>
git push origin main develop <version>
git push origin --delete release/<version>
```

## Fix production urgently

```bash
git switch main
git pull origin main
git switch -c hotfix/<short-description>
```

After testing, merge the hotfix into both permanent branches and tag the patch release:

```bash
git switch main
git merge --no-ff hotfix/<short-description>
git tag -a <version> -m "Hotfix <version>"

git switch develop
git merge --no-ff hotfix/<short-description>

git branch -d hotfix/<short-description>
git push origin main develop <version>
git push origin --delete hotfix/<short-description>
```

Replace placeholder values such as `<version>` with real values, for example `v1.0.0`.