# Apollo Vision Labs

This repository is the public Claude Code plugin marketplace for Apollo Vision
Labs, and it doubles as the organization level agent brief. Read it before
working in any Apollo repository.

## Who we are

Apollo Vision Labs, a software studio. Current work: a bilingual Astro marketing
site deployed on Fly.io, with a Go and Iris v14 backend planned. Public GitHub
organization: https://github.com/apollovisionlabs

## Organization rules

These apply to every Apollo repository, not just this one.

- **Language**: code, comments, commit messages, documentation and agent files
  in English. Product and website copy in French and English, kept in sync.
- **No em dashes or en dashes in prose.** Use a comma, a colon, or two
  sentences. Infrastructure resource names use hyphens only.
- **Commits**: Conventional Commits, imperative subject, no trailing period.
  Branches `feat/<topic>`, `fix/<topic>`, `docs/<topic>`, merged into `main`.
- **Tone**: say what a thing does. No marketing adjectives.

The `apollo-conventions` skill in this repository is the enforceable version of
these rules. Install it rather than restating them in each project.

## Public or private

| Goes here (public) | Goes in the private marketplace |
|---|---|
| Conventions an outside developer could adopt | Anything naming Apollo infrastructure |
| Generic framework and tooling workflows | Deployment procedures, app names, regions |
| Anything already visible in a public repository | Client work, internal process, credentials shape |

The private marketplace is
https://github.com/apollovisionlabs/apollovisionlabs-marketplace-private

When in doubt, put it in the private marketplace. Moving a plugin from private
to public is easy. The reverse is not.

## Repository layout

```
.claude-plugin/marketplace.json   Marketplace manifest, lists every plugin
plugins/<plugin-name>/
  .claude-plugin/plugin.json      Plugin manifest
  skills/<skill-name>/SKILL.md    One directory per skill
```

## Adding a plugin

1. Create `plugins/<plugin-name>/.claude-plugin/plugin.json` with at least
   `name`. Add `description`, `version`, `license` and `author` for anything
   published.
2. Add each skill as `skills/<skill-name>/SKILL.md`. Skills under `skills/` are
   discovered automatically, no manifest field needed.
3. Register the plugin in `.claude-plugin/marketplace.json` under `plugins`,
   with `name` and `source` pointing at `./plugins/<plugin-name>`.
4. Bump the plugin `version` in both manifests when you change behavior.

Never rename a published plugin in place. Add a `renames` entry in
`marketplace.json` mapping the old name to the new one.

## Writing a skill

Frontmatter carries two required fields, `name` and `description`.

- `description` states **when to use the skill only**, in the third person,
  starting with "Use when". Never summarize the skill's steps there: agents
  follow the description instead of reading the body when it does.
- Include the concrete triggers, symptoms and tool names an agent would search
  for.
- Keep the body under roughly 500 words. Move heavy reference material into a
  sibling file and link to it.
- Prefer a rationalization table and a red flags list over soft wording for any
  rule agents are tempted to skip.

## Testing before you push

```bash
# Validate every manifest parses
find . -name '*.json' -path '*.claude-plugin*' -exec sh -c 'jq . "$1" >/dev/null' _ {} \;

# Load this checkout as a local marketplace
claude
/plugin marketplace add /absolute/path/to/apollovisionlabs-marketplace
/plugin install apollo-conventions@apollovisionlabs
```

Confirm the skill appears in the session skill list before opening a PR.
