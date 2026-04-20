# Contributing to Sand Point Studios projects

Sand Point Studios collaborative-Git conventions. Apply consistently to keep
multi-contributor work safe.

## Before you start

```bash
git checkout main
git pull
git checkout -b feature/<short-description>
```

Branch prefixes: `feature/`, `fix/`, `chore/`, `docs/`, `refactor/`. Short
kebab-case names. **Never push directly to `main`.**

## While you work

- **Small, frequent pushes.** Push your branch every few hours, not at the
  end of a multi-day epic.
- **One Claude session per branch + file.** Two concurrent `claude` runs
  editing the same file race; pick one.
- **Open a Draft PR early.** Signals "claimed and in flight" so others
  don't pick something overlapping:
  ```bash
  gh pr create --draft --fill
  ```
  Convert to Ready for Review when you're done.

## Before you push

```bash
git pull --rebase origin main
```

Surfaces conflicts in your terminal (resolvable in real time) instead of
in the PR's "this branch has conflicts" red banner. Keeps history linear.

## Reviews

- `.github/CODEOWNERS` auto-requests reviewers based on what files you touched.
- Default product code: either Teagan or Lewis can review.
- Always Teagan: DB migrations (`/alembic/`), CI/CD (`/.github/`), dependency
  manifests (`pyproject.toml`, `package.json`), billing/auth code,
  `CLAUDE.md`, `DECISIONS.md`.
- Wait for at least one approval (you can't approve your own PR).
- Address review comments by pushing additional commits to your branch
  (don't force-push after a review starts unless asked).

## Merging

- **Squash-merge by default** (cleaner history; one commit per PR on `main`).
- Rebase-merge only when the branch is already in clean atomic commits.
- Delete the branch after merge (GitHub does this automatically if the
  setting is on).

## Commits + PR titles

Conventional Commits format:

- `feat: add avatar upload to character creator`
- `fix: voice latency >5s on 70b model`
- `chore: bump sentry-sdk to 2.20`
- `docs(api): clarify rate-limit headers`
- `refactor(api): extract dice service from session router`

PR title should be the same shape (one-line summary, optionally with scope).

## Co-author

If Claude Code wrote significant portions of the change, add the
co-author trailer to the commit message:

```
Co-Authored-By: Claude Opus 4.7 (1M context) <noreply@anthropic.com>
```

## When something goes wrong

- Conflicts on `git pull --rebase`: resolve them in the editor, `git add` the
  resolved files, `git rebase --continue`.
- Pushed to wrong branch: tell Teagan, don't try to clean it up alone.
- Accidentally force-pushed and lost commits: Teagan can recover from
  GitHub's reflog; ask before doing anything else.

## Questions

Ask Teagan. Include: what you were trying to do, what command you ran, the
exact error.
