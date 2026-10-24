# Skills Specification & Registry for AI Agents
> **Project 2027** — Agent-Native PHP/SQLite CMS by **Tabson.ru Studio**

This directory contains executable skills and declarations for AI Agents (Cursor, Claude, OpenAI Agent, Windsurf) working with the **Agent-Native PHP/SQLite CMS**.

---

## 🤖 What are Skills in Project 2027?

In **Project 2027**, a **Skill** is a self-contained action protocol or workflow defined inside a module (`/modules/<module_name>/skills/*.json`). Skills allow AI Agents to interact with the CMS without needing to inspect underlying PHP code or construct raw SQL queries.

Skills are automatically discovered by `core/SkillRegistry.php` and exposed through the **Model Context Protocol (MCP)** at `/admin/mcp.php`.

---

## 📁 Skill Directory Architecture

```text
public_html/
└── modules/
    ├── <module_name>/
    │   ├── manifest.json            # Module metadata
    │   ├── handler.php              # PHP hooks (EventBus listeners)
    │   └── skills/                  # AI Skill definitions
    │       ├── <skill_name>.json    # Declarative skill schema
    │       └── README.md            # Skill usage documentation
```

---

## 🛠 Skill JSON Standard Specification

Every skill JSON file placed inside `/modules/<module_name>/skills/` must follow this standard schema:

```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "skill_id": "pawnshop_calculator_manager",
  "version": "1.0.0",
  "title": "Manage Pawnshop Loan Rates",
  "description": "Allows AI Agents to read, adjust, and recalculate gold/silver pawn loan rates dynamically.",
  "module": "pawnshop_calc",
  "actions": [
    {
      "action": "update_rates",
      "description": "Update gold/silver buyback rates per gram across all calculator blocks.",
      "parameters": {
        "gold_585": "number (price per gram in RUB)",
        "gold_999": "number (price per gram in RUB)",
        "silver_925": "number (price per gram in RUB)"
      },
      "mcp_tool": "update_block_data"
    }
  ]
}
```

---

## ⚙️ How AI Agents Use Skills

1. **Discovery:** When calling `get_site_structure` via MCP (`/admin/mcp.php`), the `SkillRegistry` automatically aggregates all available skills across active modules.
2. **Execution:** The agent maps the required human intent (e.g., *"Change gold rate to 4100 RUB"*) to a registered skill action.
3. **Validation:** Submissions are validated against `SchemaValidator` before updating SQLite records (`pages` or `settings`).
4. **Time-Travel Logging:** Every skill action executed by an AI Agent creates an immutable snapshot record in the `revisions` table (`actor_type = 'agent'`).

---

## 🚀 Available Built-In Core Skills

| Module / Skill ID | Description | MCP Action |
| :--- | :--- | :--- |
| `telegram_leads` | Manage lead dispatch rules & Telegram bot credentials | `set_telegram_credentials` |
| `pseo_generator` | Batch generate programmatically optimized landing pages | `batch_create_pages` |
| `revision_control` | Roll back site content to a previous snapshot | `rollback_revision` |

---

## 📝 Best Practices for Developers

- **One Responsibility Per Skill:** Keep skill JSON schemas tight and focused on a single business capability.
- **Strict Parameter Types:** Always define clear data types (`string`, `number`, `boolean`, `array`) in action parameters.
- **No Hardcoded Credentials:** Never store tokens or keys inside skill JSON files. Store references pointing to the `settings` table.

---

Developed with ❤️ by **Tabson.ru Studio**
