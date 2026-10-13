# agent-plugins

Agent skills shared across [autobutler-org](https://github.com/autobutler-org) repositories, packaged as a
[Claude Code plugin marketplace](https://code.claude.com/docs/en/plugin-marketplaces). One copy here replaces a
copy in every repository.

## What is in it

One plugin, `autobutler`:

| Skill                      | What it does                                                               |
| -------------------------- | -------------------------------------------------------------------------- |
| `autobutler:resolve-issue` | Takes a GitHub issue from diagnosis to an open pull request                 |
| `autobutler:gh-stack`      | Manages stacked PRs with [gh-stack](https://github.com/github/gh-stack)     |

The skills carry the conventions every repository in the org shares: conventional commits, signed-off commits,
stacked PRs, no AI attribution. Anything specific to one repository — which checks to run, what is generated —
stays in that repository's `AGENTS.md`, which the skills defer to.

## Use it in a repository

Commit this as `.claude/settings.json`. Everyone who opens the repository in Claude Code is then offered the
marketplace and the plugin:

```json
{
  "extraKnownMarketplaces": {
    "autobutler-org": {
      "source": {
        "source": "github",
        "repo": "autobutler-org/agent-plugins"
      }
    }
  },
  "enabledPlugins": {
    "autobutler@autobutler-org": true
  }
}
```

Or install it for yourself:

```text
/plugin marketplace add autobutler-org/agent-plugins
/plugin install autobutler@autobutler-org
```

## Other agents

Each skill is a plain directory with a `SKILL.md` under `plugins/autobutler/skills/`. An agent that reads skills
from a directory (`.agents/skills/`, for example) can use a copy or a symlink of one; only the `autobutler:`
prefix is specific to Claude Code.

## Add a skill

1. Create `plugins/autobutler/skills/<name>/SKILL.md` with `name` and `description` frontmatter.
2. Keep it repository-neutral: point at the consuming repository's `AGENTS.md` for its commands and rules.
3. Run `claude plugin validate . --strict` and `claude plugin validate plugins/autobutler --strict`.
4. Bump `version` in `plugins/autobutler/.claude-plugin/plugin.json` and add a row to the table above.

## License

[MIT No Attribution](LICENSE). `plugins/autobutler/skills/gh-stack/` is vendored from
[github/gh-stack](https://github.com/github/gh-stack) under [its MIT license](plugins/autobutler/skills/gh-stack/LICENSE).
