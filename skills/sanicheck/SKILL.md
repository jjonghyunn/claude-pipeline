---
name: sanicheck
description: Scan staged/changed files for sanitize rule violations and fix them before committing. Argument "fix" applies replacements without confirmation.
---

# /sanicheck — Sanitize Rule Checker

Scan staged git files (or changed files if nothing is staged) for sanitize rule violations, report them, and optionally fix them.

## Sanitize Rules

**The rule table itself is deliberately NOT stored in this repository.**

A sanitize rule table has to spell out `<real value> → <placeholder>` pairs, so the
document that describes the rules is the one thing sanitizing cannot protect —
publishing it hands anyone the key to reverse every placeholder in every other repo.
Keep it local.

Read the live table from the user's private global instructions at
`~/.claude/CLAUDE.md` (section: **Commit Sanitize Rules**). It is loaded into context
automatically, and it is the single source of truth — this skill must never carry its
own copy that can drift or leak.

The table there covers these categories:

| Category | Placeholder shape |
|---|---|
| Employer / client company names | `company_name` |
| Team, department, and unit names | `team_name`, `part_name` |
| Campaign names and their variants | `CAMPAIGN NAME`, `campaign_name` |
| OS account name and personal user ids | `user_name`, `user_id` |
| Internal business jargon and site codes | neutral equivalents |
| Business-division codes | `DIV1`, `DIV2`, … |
| Analytics identifiers (project/segment/report-suite/login ids) | `YOUR_*` tokens |

Beyond the literal table, also apply the regex patterns defined in the same section
(analytics identifiers, 24-hex component ids, numeric login ids) and the case-sensitive
residue checks.

## Steps

1. Run `git diff --cached --name-only` to get staged files. If none, use `git diff --name-only HEAD`.
2. Exclude binary files (xlsx, png, jpg, pdf, exe, zip, etc.). Check only text files (py, md, txt, sql, bat, vbs, json, csv, etc.).
3. Read each file and detect any original strings from the rule table in `~/.claude/CLAUDE.md`, plus the regex patterns listed there.
4. Report violations with file path and line number.
5. Check file and folder **names** too, not just contents — product codenames and campaign abbreviations leak through paths.

## Arguments

- No argument: Report violations, then ask user for confirmation before fixing.
- `fix`: Apply all replacements immediately without confirmation.

## Output Format

No violations: `✓ No sanitize rule violations found`

Violations found:
```
⚠ Sanitize rule violations detected:
- path/to/file.py:12 → <matched token> (→ user_name)
- path/to/file.md:5  → <matched token> (→ company_name)
...
Total: N item(s)
```

Print the token that actually matched; never restate the rule table in full.

After fix: `✓ N item(s) fixed`

## Notes

- Commit messages are not checked by this skill — apply rules to commit messages manually.
- Local-only files not tracked by git do not need to be sanitized.
- Never write the rule table — or any original→placeholder pair — into a file that gets
  committed to a public repository, including this skill's own documentation.
