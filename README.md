# WyeWorks Claude Code plugins

The marketplace catalogue for [Claude Code](https://claude.com/claude-code) plugins built at
WyeWorks. This repository holds no plugin code — only
[`.claude-plugin/marketplace.json`](.claude-plugin/marketplace.json), which points at the repository
each plugin lives in.

Both this catalogue and the plugins it lists are **private** for now, so installing requires git
access to the WyeWorks organisation.

## Use it

Inside Claude Code:

```
/plugin marketplace add wyeworks/claude-plugins
/plugin install accountable-review@wyeworks
```

Or from a terminal:

```bash
claude plugin marketplace add wyeworks/claude-plugins
claude plugin install accountable-review@wyeworks
```

Install at project scope instead, to declare the plugin in a repository's own settings so everyone
working on it gets the same set:

```bash
claude plugin marketplace add wyeworks/claude-plugins --scope project
claude plugin install accountable-review@wyeworks --scope project
```

Scopes are `user` (default, every project), `project` (checked in, shared with collaborators), and
`local` (this machine, this project, uncommitted).

## What is listed

| Plugin | Repository | What it does |
|---|---|---|
| `accountable-review` | [wyeworks/accountable-review](https://github.com/wyeworks/accountable-review) | Turns a pull request into a published review map a reviewer can read before judging the change |

## Adding a plugin

Append an entry to the `plugins` array. The shape is a name, a source, and a description:

```json
{
  "name": "your-plugin",
  "source": { "source": "github", "repo": "wyeworks/your-plugin" },
  "description": "One line, in the voice a user reads in the plugin list."
}
```

Then validate before pushing — this checks the file as a *marketplace* manifest, not as a plugin:

```bash
claude plugin validate .
```

**Keep `version` out of catalogue entries.** Each plugin's own `.claude-plugin/plugin.json` is the
single source of truth, and it wins when both are set. Pin `ref` or `sha` in the entry only when a
release deliberately needs holding back.

Worth testing the entry before it is public: `claude plugin marketplace add` accepts a local path, so
you can add this directory, install from it, confirm the plugin's commands resolve, then
`claude plugin marketplace remove wyeworks`.
