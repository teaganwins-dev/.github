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

---

## For Claude Code (and other AI agents) working in this repo

Humans can skip this section; it's for the AI contributors.

**Check before asking.** Before asking the user for any credential, URL, host,
API token, password, SSH target, webhook secret, or access path, check the
existing sources and try the operation yourself:

- **Bitwarden is auto-unlocked** on Teagan's workstation — `BW_MASTER_PASSWORD`
  is set as a persistent Windows user env var, and as a shell env on
  vigil-server. Do not ask the user to unlock BW.
  ```bash
  [ -z "$BW_MASTER_PASSWORD" ] && export BW_MASTER_PASSWORD=$(powershell.exe -NoProfile -Command '[Environment]::GetEnvironmentVariable("BW_MASTER_PASSWORD","User")' | tr -d '\r\n')
  bw unlock --passwordenv BW_MASTER_PASSWORD --raw > /tmp/.bw_session
  export BW_SESSION=$(cat /tmp/.bw_session)
  bw list items --search "<term>"
  ```
- **SSH is pre-keyed** on every owned host — `vigil-desk`, `vigil-server`,
  `plex-server`, `yeti-nas` (user `admin`), the six VTC code-server containers.
  Try `ssh` before asking for access.
- **Dev-context + memory** — `/shared/dev-context/` (in containers) or
  `~/dev-context/` (on Teagan's laptop) has every platform, tool, project, and
  decision. Read before asking where something lives.
- **Authenticated CLIs** — `wrangler`, `flarectl`, `gh`, `az`, `railway`,
  `supabase`, `eas`, `expo`, `bw`, `docker`, `ssh`. Use them directly. Never
  send the user to a dashboard for work these cover.

**"Execute with care" does not gate low-risk reads.** Retrieving a credential
from an unlocked vault, SSH-ing to a host in your key-ring, listing DNS
records, or inspecting a queue are reads — the care bias applies to
*destructive* actions (force-push, delete, write to shared state), not to
lookups. If you're composing a "do you have X handy?" or "can you get me
access to Y?" message, **stop, check the obvious sources, and try.**

Teagan has flagged "agents asking for things they already have access to" as
the single biggest daily friction point. Don't be that agent.
