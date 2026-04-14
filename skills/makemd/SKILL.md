---
name: makemd
description: Read a Python script and auto-generate a Korean markdown documentation file in a structured table format.
---

# /makemd — Python Script Doc Generator

Read a Python script and generate a Korean markdown documentation file.

## Steps

1. Read the script at the path given in `$ARGUMENTS`. If no path is given, ask the user.
2. Analyze the full code to identify:
   - Purpose and role of the script
   - Main processing flow (input → processing → output)
   - Configuration values (constants, paths, parameters at the top)
   - Key functions/classes and their roles
   - Output files and their format
   - Error/exception handling paths
3. Generate a markdown file following the format below.
4. Save as `{script_name}_notes.md` in the same folder as the script.
   - If a file with that name already exists, ask the user before overwriting.

## Output Format

```markdown
# {script filename}

## 개요
{1~2 line summary in Korean}

## 처리 흐름

\`\`\`
{input file or source}
  ↓ {processing step 1}
  ↓ {processing step 2}
  → {output file or result}
\`\`\`

## 설정값

| 항목 | 기본값 | 설명 |
|---|---|---|
| ... | ... | ... |

## 주요 함수

| 함수 | 역할 |
|---|---|
| ... | ... |

## 출력 형식

{output columns or file structure}

## 예외 상황

| 상황 | 동작 |
|---|---|
| ... | ... |
```

## Style Guidelines

- Write in Korean.
- Use tables extensively.
- Express processing flow with `↓` arrows.
- Keep it concise — practical reference level, not exhaustive.
- Apply sanitize rules (CNXK→user_name, Concentrix→company_name, etc.).
