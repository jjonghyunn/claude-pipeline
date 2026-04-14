---
name: sanicheck
description: Scan staged/changed files for sanitize rule violations and fix them before committing. Argument "fix" applies replacements without confirmation.
---

# /sanicheck — Sanitize Rule Checker

Scan staged git files (or changed files if nothing is staged) for sanitize rule violations, report them, and optionally fix them.

## Sanitize Rules

| Original | Replace with |
|---|---|
| `CNXK` | `user_name` |
| `Concentrix Corporation` | `company_name` |
| `Concentrix` | `company_name` |
| `삼성` | `company_name` |
| `KR_Digital Intelligence` | `team_name` |
| `GMO Demand Generation` | `part_name` |
| `MOTHERS DAY` | `CAMPAIGN NAME` |
| `The Frame` | `CAMPAIGN NAME` |
| `TheFrame` | `CAMPAIGN_NAME` |
| `theframe` | `campaign_name` |
| `더프레임` | `CAMPAIGN NAME` |
| `GMO DG` | `part_name` |
| `pjh` | `user_id` |
| `_md_` | `_` |

## Steps

1. Run `git diff --cached --name-only` to get staged files. If none, use `git diff --name-only HEAD`.
2. Exclude binary files (xlsx, png, jpg, pdf, exe, zip, etc.). Check only text files (py, md, txt, sql, bat, vbs, json, csv, etc.).
3. Read each file and detect any original strings from the table above.
4. Report violations with file path and line number.

## Arguments

- No argument: Report violations, then ask user for confirmation before fixing.
- `fix`: Apply all replacements immediately without confirmation.

## Output Format

No violations: `✓ No sanitize rule violations found`

Violations found:
```
⚠ Sanitize rule violations detected:
- path/to/file.py:12 → 'CNXK' (→ user_name)
- path/to/file.md:5  → 'Concentrix' (→ company_name)
...
Total: N item(s)
```

After fix: `✓ N item(s) fixed`

## Notes

- Commit messages are not checked by this skill — apply rules to commit messages manually.
- Local-only files not tracked by git do not need to be sanitized.
