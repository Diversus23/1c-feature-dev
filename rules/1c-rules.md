## 1C Coding Rules

### Comments
- Prefer self-documenting code over comments. Avoid comments that simply repeat what the code does.
- Comments are appropriate only when they add value: explaining motivation, describing non-trivial algorithms or constraints/side effects, marking technical debt, or providing context that cannot be expressed clearly in code.

### Code Quality
- After any code edit, perform an internal code review (style, readability, correctness, edge cases, security, concurrency, best practices). If issues are found, fix them and repeat the cycle until the code is clean, correct, and stable.
- Check code for concurrency issues and proper use of locks/transactions. Always consider whether an external transaction exists (e.g., object write transaction).
- Module region purposes: `ПрограммныйИнтерфейс` — public, `СлужебныйПрограммныйИнтерфейс` — private, `СлужебныеПроцедурыИФункции` — protected
- Before writing code, check common modules and manager modules for existing export methods to avoid duplicating logic.
- When writing queries, verify metadata attributes (existence, names, types) used in the query.
- Use the following query formatting (query text on a new line, at the same indentation level as `Запрос = Новый Запрос;`):
```bsl
Запрос = Новый Запрос;
Запрос.Текст =
    "текст запроса";
```
- Do not use `Сообщить()`. Use `ОбщегоНазначения.СообщитьПользователю` (server-side) or `ОбщегоНазначенияКлиент.СообщитьПользователю` (client-side) instead.
- Use a 120-character line limit where the line can be correctly wrapped.
- In form modules, minimize client-server round trips.
- In form modules, methods can have the following compilation directives: `&НаКлиенте`, `&НаСервере` (with form context), `&НаСервереБезКонтекста` (without form context — preferred, as it does not transfer form context to the server), `&НаКлиентеНаСервереБезКонтекста`
- In form modules, prefer `Асинх` over `ОписаниеОповещения` where an async equivalent method exists.
- Always use an intermediate variable for query results: `РезультатЗапроса = Запрос.Выполнить()`. Do not chain calls like `Запрос.Выполнить().Выгрузить()` or `Запрос.Выполнить().Выбрать()`.
- Always alias query fields with `КАК`, e.g. `Контрагенты.ИНН КАК ИНН`.
- Avoid queries inside loops. Use batch queries and temporary tables instead.
- Cache repeated calculations in loops (catalog lookups, attribute retrieval, same-algorithm computations) using `Соответствие` for performance optimization.
- Do not access reference attributes via dot notation — this fetches the entire object from the database. For example: `Контрагент.ИНН` — incorrect (if `Контрагент` is a reference), `ОбщегоНазначения.ЗначениеРеквизитаОбъекта(Контрагент, "ИНН")` — correct. Available methods: `ОбщегоНазначения.ЗначениеРеквизитаОбъекта`, `ОбщегоНазначения.ЗначениеРеквизитовОбъекта`, `ОбщегоНазначения.ЗначениеРеквизитаОбъектов`
- Do not use ternary operators: `ИНН = ?(Контрагент.ИНН <> "", Контрагент.ИНН, "Не указан")`
- Do not use Hungarian notation for variables. Example: `МассивКонтрагентов` — incorrect, `Контрагенты` — correct.
- Do not use 1C global context names for variables (`Документы`, `Справочники`, `Пользователи`, `Регистры`, `Метаданные`, `Константы`, etc.). This creates name collisions and reduces readability.
- Do not use `Попытка...Исключение` for database read/write operations without justified transaction management needs.
- Do not use `ЗаписьЖурналаРегистрации()` unless explicitly requested by the user.
- Do not compare boolean values to `Истина`/`Ложь` — use boolean expressions directly. Example: `Если Активен Тогда` instead of `Если Активен = Истина Тогда`.
- Use query parameters (`Запрос.УстановитьПараметр()`) instead of string concatenation to prevent injection and improve performance.
- Use `ПЕРВЫЕ N` when only a limited number of records is needed.
- Use `УстановитьПривилегированныйРежим(Истина)` sparingly and only when necessary. Always disable after the operation: `УстановитьПривилегированныйРежим(Ложь)`.
- Use `ЗаполнитьЗначенияСвойств()` for bulk property assignment.
- For lookups in large collections, use `Соответствие` (O(1)) instead of array iteration (O(n)).
- Document public procedures and functions: purpose, parameters, return value.

### Formatting
- Do not break a line if only a single variable would remain on the new line.
- Add blank lines before and after code inside condition and loop blocks for readability.
