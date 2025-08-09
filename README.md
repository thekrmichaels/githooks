# Git Hooks + Notion Integration

Automates commit messages and task tracking using Notion database.

## Setup

1. Setup Notion Integration:

   a. **Duplicate the database template**: [Task Database Template](https://caramel-algebra-a5c.notion.site/22cd9d4526b2808b9bf3ec11076b48ac?v=22cd9d4526b281599e2c000c36de1158)

   b. **Create integration**:
      - Go to [notion.so/my-integrations](https://www.notion.so/profile/integrations)
      - Click "New integration"
      - Name it and select your workspace
      - Copy the **Internal Integration Token**

   c. **Connect database**:
      - Open your duplicated database
      - Click **⋯** → **Add connections**
      - Select your integration

   d. **Get Database ID** from your database URL:

      ```text
      https://notion.so/workspace/DATABASE_ID?v=VIEW_ID
      ```

2. Install `jq` (required for JSON parsing):

   ```bash
   # Ubuntu/Debian
   sudo apt install jq
   
   # macOS
   brew install jq
   
   # Windows
   winget install jqlang.jq
   ```

3. Copy hooks to `.git/hooks/`:

   ```bash
   cp hooks/* .git/hooks/
   chmod +x .git/hooks/prepare-commit-msg .git/hooks/post-commit
   ```

4. Create `.env` in project root:

   ```env
   NOTION_TOKEN=your_notion_token
   DATABASE_ID=your_database_id
   ```

## Usage

Commit with `@task:<id>` to auto-generate conventional commit from Notion task:

```bash
git commit -m "@task:ABC123"
```

The hook will:

- Fetch task details from Notion
- Generate conventional commit message
- Mark task as done after commit

## Notion Database Schema

Required properties:

- `Task ID` (Title)
- `type` (Select)
- `description` (Rich Text)
- `Done` (Checkbox)

Optional:

- `gitmoji` (Select)
- `scope (optional)` (Rich Text)
- `body (optional)` (Rich Text)
- `footer(s) (optional)` (Rich Text)
