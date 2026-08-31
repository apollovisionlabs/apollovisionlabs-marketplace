# READMEs in Open Knowledge Format

Every `README.md` in an Apollo Vision Labs repository is an OKF concept
document: YAML frontmatter, then a markdown body. OKF is a vendor neutral
format for knowledge as plain markdown, specified at
https://github.com/GoogleCloudPlatform/knowledge-catalog/tree/main/okf

We follow OKF v0.2. This file records the subset we require and the choices
the spec leaves open.

## Frontmatter

`type` is the only key OKF always requires. We require four more, because a
README with no owner, no date and no one sentence summary is the thing this
convention exists to prevent.

```yaml
---
type: Project
title: Apollo Audit
description: Confronts a health facility document corpus to a certification referential and reports the gaps.
resource: https://github.com/apollovisionlabs/apollo-audit
status: draft
generated:
  by: human:remydeme
  at: 2026-08-31T09:00:00Z
---
```

| Key | Required | Notes |
|---|---|---|
| `type` | yes | One of the values below. |
| `title` | yes | Display name. Not the repository slug. |
| `description` | yes | One sentence, ends with a period, says what the thing does. |
| `resource` | yes | The canonical URI of the asset the README describes. |
| `generated` | yes | `by` is required inside it, `at` is an ISO 8601 instant with an explicit UTC offset. |
| `status` | yes | `draft`, `stable` or `deprecated`. OKF defaults to `stable`, we write it out. |
| `tags` | when useful | YAML list, lowercase, for cross cutting search. |
| `verified` | when reviewed | See trust below. |
| `stale_after` | when time bound | ISO 8601 instant after which the content is stale. |
| `sources` | when derived | List of `{resource, title}` the content was built from. |

## Type values

OKF does not register type values centrally. Ours:

| Value | For |
|---|---|
| `Monorepo` | A repository holding several projects. |
| `Project` | A deployable application or service. |
| `Library` | A package consumed by other code. |
| `Plugin` | A Claude Code plugin or marketplace. |
| `Reference` | Documentation with no deployable artifact. |

Add a value rather than stretching an existing one, and record it here.

## Actors

OKF's actor convention, which we adopt as written:

- `human:<id>` for a person, the id being the GitHub handle.
- `<producer>/<version>` for an agent, for example `claude/opus-5`.
- `process:<id>` for an automated process.

## Trust

OKF derives a trust tier from `verified`. No `verified` key means unverified.
Verified by non human actors only means machine confirmed. Verified by a
`human:<id>` actor means human reviewed.

A README written by an agent carries `generated.by: claude/<model>` and gains a
`verified` entry only once a person has actually read it:

```yaml
generated:
  by: claude/opus-5
  at: 2026-08-31T09:00:00Z
verified:
  - by: human:remydeme
    at: 2026-08-31T14:20:00Z
```

Never write a `verified` entry for a human who has not reviewed the file. That
key is the only signal separating a draft from a reviewed document, and forging
it makes every other README worthless.

## Body

Standard markdown. OKF gives `# Schema`, `# Examples` and `# Computation` a
defined meaning, so use those headings only for what the spec says they carry.
Everything else is free.

The body still follows the house rules: no em dashes, no en dashes, no
marketing adjectives, English.

## Reserved filenames

`index.md` and `log.md` are reserved by OKF and cannot be concept documents.

- `index.md` lists the directory contents as `* [Title](relative-url) - short description`. Only the bundle root `index.md` carries frontmatter, and only `okf_version: "0.2"`.
- `log.md` holds update history, newest first, grouped under ISO 8601 date headings.

Neither is required. Add them when a documentation directory grows past the
point where a reader can scan it.

## Cross links

Bundle relative paths beginning with `/` are the recommended form inside a
documentation bundle. Plain relative markdown links stay fine in a repository
root README, where there is no bundle root to be relative to.

## Known friction

GitHub renders YAML frontmatter in a README as a table at the top of the
repository page. This is accepted, not a bug to work around. Keep the
frontmatter short enough that the table stays readable.

## Checks

```bash
# Every README parses as YAML frontmatter plus a body
head -1 README.md | grep -qx -- '---' || echo 'README.md is missing OKF frontmatter'
```
