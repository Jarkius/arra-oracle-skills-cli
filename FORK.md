# Fork notes (Jarkius)

This clone is the source Claude Code installs `oracle-skills` from (marketplace type `directory`, set in
`~/.claude/settings.json` and `~/.claude/plugins/known_marketplaces.json`). We maintain our own patches and do not
open PRs upstream.

- `upstream` = Soul-Brews-Studio/arra-oracle-skills-cli (default branch `alpha`, not `main`)
- `origin` = Jarkius/arra-oracle-skills-cli
- Branch **`jarkius`** = `upstream/alpha` + our patches. Keep it checked out: the plugin installs the working tree.

## Workflow

```bash
# patch: edit, test (bun test __tests__/<skill>-contract.test.ts), commit on jarkius, then
git push origin jarkius
claude plugin marketplace update oracle-skills && claude plugin update oracle-skills@oracle-skills   # restart Claude Code

# take upstream
git fetch upstream && git rebase upstream/alpha && git push -f origin jarkius   # then the update line above
```

On a rebase conflict, check whether upstream fixed the same thing; if so, drop our commit.

## Our patches

| Commit | Skill | What and why |
|---|---|---|
| fix(rrr): session clock and GMT+7 stamps work on Windows | rrr | `session-clock.py` encoded the cwd POSIX-only, so on Windows it reported `evidence: none`; now every non-alphanumeric char -> `-` like Claude Code. ASCII output (Windows grep saw binary). SKILL.md `TZ='ICT-7'`: Git Bash has no tz data, so `Asia/Bangkok` silently became GMT. |
