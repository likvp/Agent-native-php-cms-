# Auto-Migrator Skill: Zero-Touch PHP/MySQL to Project 2027 Pipeline
> **Module:** `auto_migrator` | **Vendor:** Tabson.ru Studio  
> **Workflow:** Upload $\rightarrow$ Auto-Scan Legacy Files & DB $\rightarrow$ Migrate to `/new/` $\rightarrow$ Staging Admin Approval

This document defines the AI Agent Skill specification for the automated end-to-end migration engine. The CMS is uploaded directly to a server containing a legacy PHP 5.x–8.x / MySQL site, automatically detects legacy config files, extracts database credentials, builds a parallel installation in `/new/`, and waits for human approval in the staging dashboard before going live.

---

## 🤖 Skill Declaration (`/modules/auto_migrator/skills/auto_migrator.json`)

```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "skill_id": "auto_migrator_pipeline",
  "version": "1.0.0",
  "title": "Automated Legacy PHP/MySQL Discovery & Migration Engine",
  "description": "Scans parent host directory for legacy PHP/CMS configs (WordPress, Bitrix, OpenCart, Custom), extracts MySQL connection credentials, parses posts/pages, constructs Project 2027 SQLite DB in /new/ directory, and locks staging mode awaiting admin approval.",
  "module": "auto_migrator",
  "actions": [
    {
      "action": "run_environment_scan",
      "description": "Scans root web directory for legacy configuration files (wp-config.php, db.php, config.php, dbconn.php) and extracts MySQL credentials.",
      "parameters": {
        "root_path": "string (Directory path to scan, default: '../')"
      },
      "mcp_tool": "scan_legacy_environment"
    },
    {
      "action": "build_staging_environment",
      "description": "Creates /new/ directory structure, initializes new data.sqlite database, and converts MySQL tables into Bento JSON pages.",
      "parameters": {
        "target_dir": "string (Default: '/new')",
        "overwrite_staging": "boolean (Default: true)"
      },
      "mcp_tool": "create_staging_instance"
    },
    {
      "action": "swap_to_production",
      "description": "Executes final migration swap upon admin approval: moves legacy site to /legacy_backup and moves /new content to root /.",
      "parameters": {
        "backup_legacy": "boolean (Default: true)"
      },
      "mcp_tool": "approve_and_swap_production"
    }
  ]
}
```

---

## 📋 Complete Automated Workflow Architecture

```text
[Legacy PHP/MySQL Site in Root /]
       │
       ├── 1. Upload Project 2027 Installer ZIP to /new/
       │
[Project 2027 in Staging Mode (/new/)]
       │
       ├── 2. Auto-Detects Legacy Config (wp-config.php, etc.)
       ├── 3. Connects to MySQL & Extracts Content
       ├── 4. Transforms HTML $\rightarrow$ Bento JSON $\rightarrow$ SQLite (data.sqlite)
       │
[Staging Admin Dashboard (/new/admin/)]
       │
       ├── 5. Human Reviews Site in Staging Mode
       └── 6. Click "Approve & Go Live" $\rightarrow$ Atomic File Swap (/new/ $\rightarrow$ /)
```

---

## 🛠 Step-by-Step AI Agent Execution Protocol

### Step 1: Environment & Credential Auto-Discovery
The module inspects parent directories (`../`) relative to `/new/` to locate existing CMS footprints:
* **WordPress:** Parses `../wp-config.php` for `DB_NAME`, `DB_USER`, `DB_PASSWORD`, `DB_HOST`, `$table_prefix`.
* **Bitrix:** Parses `../bitrix/php_interface/dbconn.php`.
* **OpenCart / Joomla:** Parses `../config.php` / `../configuration.php`.
* **Custom Legacy PHP:** Scans for `PDO` or `mysqli_connect` patterns in root `.php` files.

### Step 2: In-Memory Conversion & Asset Offloading
1. Connects to source MySQL database via PDO using discovered credentials.
2. Reads legacy articles, pages, categories, and custom fields.
3. Converts raw HTML into structured **Bento JSON** blocks.
4. Copies image attachments from `../uploads/` or `../wp-content/uploads/` to `/new/assets/uploads/`, running `MediaManager::processAndCompress()` to convert them to `.webp`.

### Step 3: Staging SQLite Population (`/new/data.sqlite`)
1. Creates `/new/data.sqlite` in `WAL` mode.
2. Writes converted pages into `pages` table.
3. Sets site state in `settings` table: `migration_status = 'pending_approval'`.
4. Renders live preview of the converted site under `http://domain.com/new/`.

### Step 4: Admin Staging Approval & Atomic Swap
When the user enters `/new/admin/` and clicks **«Одобрить и запустить в продакшен»**:
1. Creates backup of current root: Moves root files to `/backup_legacy_[timestamp]/`.
2. Moves contents of `/new/` to root `/`.
3. Flushes static cache (`CacheManager::clear()`).
4. Triggers `SeoManager::pingIndexNow()` for instant search re-indexing.

---

Developed with ❤️ by **Tabson.ru Studio**
