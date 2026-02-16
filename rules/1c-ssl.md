## SSL (БСП / BSL Standard Library) Rules

### Core Principle

**ALWAYS check if SSL / БСП has an existing solution before writing custom code.** Writing custom code when SSL (БСП) provides a solution is technical debt.

### Workflow for New Functionality

1. **Search SSL first** — use available MCP-tools for:
   - Search for common BSL modules by name
   - Get module details (export methods, properties)
   - Search for examples of usage in the code base
2. **Check existing patterns** — search through MCP or by file search, to find similar tasks solved in the project
3. **Use SSL if available** — it is tested, optimized, and maintained
4. **Only then write custom code** — and document why SSL was not suitable

### Key SSL Modules

- `Пользователи` — users, roles, access rights
- `РаботаСФайлами` — file storage and attachments
- `УправлениеПечатью` — print forms
- `ДлительныеОперации` — background jobs with progress
- `ВерсионированиеОбъектов` — object versioning history
- `РаботаСПочтовымиСообщениями` — email sending
- `ОбщегоНазначения` / `ОбщегоНазначенияКлиентСервер` — common utilities
- `СтроковыеФункцииКлиентСервер` — string functions
