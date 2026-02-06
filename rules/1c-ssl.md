## SSL (БСП / BSL Standard Library) Rules

### Core Principle

**ALWAYS check if SSL / БСП has an existing solution before writing custom code.** Writing custom code when SSL (БСП) provides a solution is technical debt.

### Workflow for New Functionality

1. **Search SSL first** — use EDT MCP Server tools:
   - `get_metadata_objects` — find SSL common modules by name
   - `get_metadata_details` — get module details (export methods, attributes)
   - `find_references` — find usage examples in the codebase
2. **Check existing patterns** — use `find_references` to find how similar tasks are solved in the project
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
