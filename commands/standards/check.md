---
description: Check the backend skill against ko3 global standards.
argument-hint: "[skill-path]"
---

Inspect the backend skill (default: `skills/backend/SKILL.md`, or `$1` if supplied) and verify it against `standards/global/README.md`.

Specifically:

1. Read the skill's frontmatter and confirm every required field is present and non-`TODO`: `name`, `description`, `owner`, `backup`, `status`, `version`, `created`, `last_reviewed`, `plugin_version`, `changelog`.
2. Confirm `last_reviewed` is within the staleness window defined in `standards/global/README.md` (flag if missing or older than the threshold).
3. Confirm `plugin_version` matches the current `version` in `.claude-plugin/plugin.json`.
4. Read the skill body and check it follows the structure required by the global standards (headings, examples, "when to use" guidance).
5. Report findings as a prioritized list — blockers first, then warnings, then suggestions. If everything passes, say so explicitly.
