---
name: apollo-conventions
description: Use when writing or editing any prose, documentation, README, commit message, branch name, website copy, or resource name in an Apollo Vision Labs repository, and when reviewing text someone else wrote for those repositories.
---

# Apollo Conventions

House rules for anything written in an Apollo Vision Labs repository. They cover
prose and naming, not application logic.

## The dash rule

**Never emit an em dash (U+2014) or an en dash (U+2013) in prose.** Not in
source files, documentation, commit messages, website copy, PR descriptions, or
chat responses about this work. Not in infrastructure resource names, where
hyphens are the only accepted separator.

Rewrite instead:

| Instead of a dash | Write |
|---|---|
| Aside or interruption | A comma, or parentheses |
| Introducing an explanation | A colon |
| Joining two independent clauses | Two sentences, or a semicolon |
| Number or date range | `from 2024 to 2026`, or `2024-2026` with a hyphen |

Before finishing any edit to a text file, grep the changed lines for the two
characters and fix every hit:

```bash
git diff -U0 | grep -nP '^\+.*[\x{2014}\x{2013}]'
```

### Rationalizations to reject

| Excuse | Reality |
|---|---|
| "This one reads better with a dash" | Every dash reads better to whoever typed it. Rewrite the sentence. |
| "It is only a commit message" | Commit messages are the repository's permanent prose. |
| "A hyphen is close enough, I will use `-` as an aside" | A hyphen joining clauses is a dash by another name. Restructure. |
| "The user pasted a dash, so I keep it" | Quoted third party text stays verbatim. Text you author does not. |
| "I will clean the dashes up at the end" | There is no end pass. Fix them in the edit that introduced them. |

Quoting an external source verbatim is the only exception, and it stays inside
quotation marks or a code block.

## Language

- Code, comments, commit messages, documentation and agent files: English.
- Product and website copy: French and English, kept in sync.

## Tone

Say what a thing does and stop. No superlatives, no "cutting edge", no
"revolutionary", no filler adjectives. Prefer a concrete number or behavior over
an adjective. Short sentences beat long ones.

## Commits

Conventional Commits. Lowercase type, colon, then an imperative subject with no
trailing period and no dash:

```
feat: bilingual blog index
fix: missing hreflang on the contact page
docs: Fly deployment on a single machine
chore: bump Astro to 7.2.9
```

Types in use: `feat`, `fix`, `docs`, `style`, `refactor`, `test`, `chore`.

Branches: `feat/<topic>`, `fix/<topic>`, `docs/<topic>`, kebab-case topic,
merged into `main`.

## Naming

- Files and directories: kebab-case.
- Cloud and infrastructure resources: kebab-case, prefixed `apollo-`, hyphens
  only, no underscores and no dashes.
- Environment variables: SCREAMING_SNAKE_CASE.

## Red flags

Stop and fix if you catch yourself typing any of these:

- An em dash or en dash anywhere you authored.
- A commit subject that ends with a period or describes what you did rather than
  what the change does.
- Marketing adjectives in a README.
- A French sentence in a code comment, or an English-only page added without its
  French counterpart.
