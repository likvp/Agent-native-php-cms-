# Project 2027: Agent-Native PHP/SQLite CMS
> Developed by **Tabson.ru Studio**

[![PHP Version](https://img.shields.io/badge/PHP-8.1%2B-blue.svg)](https://php.net)
[![Database](https://img.shields.io/badge/SQLite-3%20(WAL)-orange.svg)](https://sqlite.org)
[![Architecture](https://img.shields.io/badge/Architecture-Modular%20Monolith-green.svg)]()
[![Agent-Native](https://img.shields.io/badge/AI-MCP%20%2F%20JSON--RPC%202.0-purple.svg)]()

**Project 2027** — это сверхлегкая, автономная и защищенная CMS нового поколения для коммерческих веб-порталов и лендингов. Создана студией **Tabson.ru** как высокопроизводительная альтернатива WordPress и Tilda с нативной поддержкой работы автономных ИИ-агентов.

---

## 🚀 Key Features & Innovations

1. **Dual-UX (Human + Agent Interface):**
   - **Human Layer:** Визуальное редактирование текста на лету (`contenteditable`) + Bento-дизайн на Tailwind CSS и Alpine.js.
   - **Agent Layer:** Полное программное управление сайтом через стандартный **MCP (Model Context Protocol)** и JSON-RPC 2.0.
2. **Zero-Setup (Self-Healing):**
   - Никаких файлов конфигурации установки. При первом запросе ядро само инициализирует базу данных SQLite в режиме **WAL (Write-Ahead Logging)**.
3. **Flat-File Shadow Cache:**
   - Компиляция страниц в статические `.html` файлы. Отдача контента за **1–2 миллисекунды** без нагрузки на PHP и SQLite.
4. **Built-in pSEO & IndexNow:**
   - Автоматическая микроразметка Schema.org (JSON-LD), генерация `/sitemap.xml` и мгновенный пинг поисковиков (Яндекс, Bing) через протокол IndexNow.
5. **Module-as-a-Skill:**
   - Модули расширения не просто добавляют код, но и передают готовые MCP-инструкции в контекст ИИ-агентов.

---

## 🛠 Tech Stack

- **Backend:** Pure PHP 8.1+ (`declare(strict_types=1);`)
- **Database:** SQLite 3 (WAL mode, FTS5 Full-Text Search)
- **Frontend:** Autonomously compiled Tailwind CSS + Alpine.js
- **Protocols:** MCP (JSON-RPC 2.0), Server-Sent Events (SSE), IndexNow

---

## 📁 Project Structure

```text
public_html/
├── index.php                 # Единый роутер, Zero-Setup и Shadow Cache
├── .htaccess                 # ЧПУ перенаправление
├── data.sqlite               # Единая БД SQLite (автосоздание)
├── AGENTS.md                 # Инструкции для ИИ-агентов
├── llms.txt                  # Markdown-сводка для LLM
│
├── core/                     # Неизменяемое ядро CMS
│   ├── Database.php          # Singleton PDO (WAL mode)
│   ├── Router.php            # Виртуальный ЧПУ-роутер
│   ├── SchemaValidator.php   # Валидация JSON-схем блоков
│   ├── SeoManager.php        # Meta, Schema.org, Sitemap & IndexNow
│   ├── CacheManager.php      # Flat-File Shadow Cache
│   ├── Streamer.php         # Server-Sent Events (SSE) Real-Time
│   ├── EventBus.php          # Шина событий и API Hub
│   └── SkillRegistry.php     # Реестр скилов для ИИ
│
├── themes/                   # Кастомные темы оформления
│   └── pawnshop_dark/        # Bento-тема по умолчанию
│
├── modules/                  # Изолированные модули (Modular Monolith)
├── assets/                   # Скомпилированные стили, скрипты и кеш
└── admin/                    # Админ-панель, REST API и /admin/mcp.php




⚡ Quick Start (Zero-Setup)
1. Клонируйте репозиторий или распакуйте архив в корень вашего веб-сервера (⁠public_html/⁠).
2. Убедитесь, что веб-сервер имеет права на запись в корневую папку (для создания ⁠data.sqlite⁠ и папки кеша).
3. Откройте ваш сайт в браузере — ядро автоматически сгенерирует структуру базы данных и дефолтную главную страницу.
4. Для входа в режим визуального редактирования перейдите по адресу ⁠/admin/⁠ (пароль по умолчанию для MVP: ⁠2027⁠).
🤖 AI Agent & MCP Integration
ИИ-агенты (Cursor, Claude, OpenAI Operator) могут управлять сайтом через стандартизированный MCP-шлюз:
 Endpoint: ⁠https://your-domain.com/admin/mcp.php⁠
 Protocol: JSON-RPC 2.0
 Available Tools: ⁠get_site_structure⁠, ⁠update_block_data⁠, ⁠batch_create_pages⁠, ⁠rollback_revision⁠.
📄 License & Studio Info
Developed with ❤️ by Tabson.ru Studio.
Licensed under the MIT License.




