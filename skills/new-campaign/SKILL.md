---
name: new-campaign
description: Create a new campaign working folder with standard structure on the local machine.
---

# /new-campaign — Campaign Folder Generator

Create a new campaign working folder locally with a standard structure.

## Steps

1. **Folder name**: Use `$ARGUMENTS` if provided. Otherwise ask the user.
   - Example format: `260407_campaign_name`, `260413_campaign_name`
   - Apply sanitize rules to the folder name if needed.

2. **Target path**: Default base path is:
   ```
   C:\Users\user_name\OneDrive - company_name\user_id\2.data\99.PY,SQL-250429\00.py_notebook\{folder_name}\
   ```
   Ask the user if they want a different location.

3. **Folder type**: Ask the user which type:
   - `schedule` — Campaign schedule management
   - `aa_exporter` — AA Exporter data extraction/refinement
   - `simple` — Basic analysis folder (script + notes.md only)
   - `custom` — User describes the structure

4. **Create folder and files**:

   **schedule:**
   ```
   {folder_name}/
   └── notes.md
   ```

   **aa_exporter:**
   ```
   {folder_name}/
   ├── aa_exports/
   ├── ref/
   └── notes.md
   ```

   **simple:**
   ```
   {folder_name}/
   └── notes.md
   ```

5. **notes.md initial content** (auto-generated):
   ```markdown
   # {folder_name}

   ## 목적
   {inferred from folder name or user input}

   ## 작업 현황
   - [ ] 

   ## 파일 목록
   | 파일 | 역할 |
   |---|---|
   |  |  |
   ```

6. Print the created path when done.

## Notes

- Warn and ask for confirmation if the folder already exists.
- This path is inside OneDrive — avoid placing large files directly here.
