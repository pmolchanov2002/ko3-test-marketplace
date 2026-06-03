---
owner: TODO
backup: TODO
status: draft
version: 0.1.0
last_reviewed: 2026-06-03
---

# ko3 Skill Coding Standards

These are the rules the `standards-checker` agent and the `/standards:check` command enforce. Every skill shipped in a ko3 plugin must comply.

## 1. Required frontmatter

Every `SKILL.md` must begin with YAML frontmatter containing **all** of the following keys. No key may be omitted, left blank, or set to the literal string `TODO`.

| Field            | Type                  | Notes                                                                              |
| ---------------- | --------------------- | ---------------------------------------------------------------------------------- |
| `name`           | string (kebab-case)   | Matches the containing folder name.                                                |
| `description`    | string                | One sentence. Must describe *what* the skill does **and** *when* to use it.        |
| `owner`          | string                | Person or team accountable for the skill.                                          |
| `backup`         | string                | Secondary contact if `owner` is unavailable.                                       |
| `status`         | enum                  | One of `draft`, `active`, `deprecated`.                                            |
| `version`        | semver                | Skill's own version. Bump on every substantive edit.                               |
| `created`        | ISO date (YYYY-MM-DD) | Set once, never changed.                                                           |
| `last_reviewed`  | ISO date (YYYY-MM-DD) | Date of the most recent human review.                                              |
| `plugin_version` | semver                | Must equal the `version` in `.claude-plugin/plugin.json` at time of last review.   |
| `changelog`      | list                  | Newest entry first. Each entry has `version`, `date`, `notes`.                     |

## 2. Staleness window

- **`last_reviewed` threshold: 90 days.**
- A skill whose `last_reviewed` is older than 90 days is a **blocker**.
- Between 75 and 90 days it is a **warning** ("stale-soon").

## 3. Plugin version coupling

`plugin_version` in the frontmatter must equal the current `version` in `.claude-plugin/plugin.json`. Any mismatch is a blocker; the reviewer must re-read the skill, update its content if necessary, then bump `plugin_version` and `last_reviewed` together.

## 4. Changelog rules

- The newest changelog entry's `version` must equal the top-level `version` field.
- Every entry has all three of `version`, `date`, `notes`.
- Entries are append-only — never rewrite history; add a new entry instead.

## 5. Required body structure

Every `SKILL.md` body must contain, in this order:

1. An `# H1` heading matching the skill's `name` (in Title Case).
2. A **When to use** section — bulleted triggers that map to the description.
3. A **When not to use** section — bulleted anti-triggers.
4. At least one **Example** — a realistic input and the expected behavior.
5. A **References** section if the skill depends on other skills, commands, or external docs.

Missing sections 1–4 are blockers. Missing section 5 is a suggestion (only when references are implied by the body).

## 6. Status semantics

- `draft` — skill is not ready for general use. Allowed in the manifest, but the checker emits a warning.
- `active` — skill is ready and supported.
- `deprecated` — skill is retained for compatibility but should not be invoked for new work. The body must explain the replacement.

## 7. TODO placeholders

The literal string `TODO` is forbidden in frontmatter values. The checker treats it as a blocker for every required field, and a warning everywhere else.
