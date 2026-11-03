# Pending release notes

This file is the **single source of truth** for the next version's changelog
entry. `./deploy.sh` reads the `## Next` section below on every version
bump, copies each bullet into `src/config/changelog.ts`, and resets the
section so the cycle can start fresh.

## The rule

> Every Claude session that adds, changes, fixes, or polishes anything a
> user can see, hear, or feel **must append at least one bullet** to the
> `## Next` section before the session wraps up.

Why: the in-app drawer Changelog is the only place most users learn what
changed. Git commit subjects are too terse (`"fixed issues"`,
`"updated package"`) to make a good release note. This file lets the
session author write the line that will actually ship to users — in plain
English, with the real "why" intact.

We never publish a "no user-facing changes" / "maintenance release"
placeholder. If a deploy attempt finds an empty `## Next` section AND no
qualifying git commits across app + kit + server, `deploy.sh` aborts.

## Format

Each bullet is `- <type>: <text>`, where `<type>` is one of:

- **feature** — a new capability the user can use that didn't exist before.
- **improvement** — an existing capability now works better, faster, or in
  more places.
- **fix** — a bug that was hurting users is gone.

Free-form lines (without the `<type>:` prefix) are accepted and default to
`improvement`. Group related bullets — one paragraph each is fine — and
write for a non-technical reader who's just looking at the in-app
changelog. Mention the screen / button / flow so it's clear where to find
the change.

A tagline can be set with a single H3 inside `## Next`, e.g.
`### tagline: Share your game leaderboards`. It's optional.

## Next


_Empty — the previous release consumed every bullet. Add new ones below as you ship work._
## How the deploy script consumes this

1. `./deploy.sh` parses the `## Next` section.
2. Each `- type: text` line becomes a `{ type, text }` entry in
   `src/config/changelog.ts` for the new version, in the order written.
3. After a successful generation, `## Next` is wiped (replaced with a
   bare heading) and that single change is committed alongside the new
   `package.json` version + the `Updated version` push. Bullets are
   permanently preserved in the changelog itself.
