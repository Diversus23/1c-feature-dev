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
- Keep procedures/functions under 200 lines; if over 100, consider decomposition. When adding new code to a common module, prefer creating a new focused module if the existing one already exceeds ~3000 lines. This applies to new code only — do not refactor existing large modules unless explicitly requested.
- In form modules, minimize client-server round trips.
- In form modules, methods can have the following compilation directives: `&НаКлиенте`, `&НаСервере` (with form context), `&НаСервереБезКонтекста` (without form context — preferred, as it does not transfer form context to the server), `&НаКлиентеНаСервереБезКонтекста`
- In form modules, prefer `Асинх` over `ОписаниеОповещения` where an async equivalent method exists.
- Always use an intermediate variable for query results: `РезультатЗапроса = Запрос.Выполнить()`. Do not chain calls like `Запрос.Выполнить().Выгрузить()` or `Запрос.Выполнить().Выбрать()`.
- Always alias query fields with `КАК`, e.g. `Контрагенты.ИНН КАК ИНН`.
- Avoid queries inside loops. Use batch queries and temporary tables instead.
- Cache repeated calculations in loops (catalog lookups, attribute retrieval, same-algorithm computations) using `Соответствие` for performance optimization.
- Do not access reference attributes via dot notation — this fetches the entire object from the database. For example: `Контрагент.ИНН` — incorrect (if `Контрагент` is a reference), `ОбщегоНазначения.ЗначениеРеквизитаОбъекта(Контрагент, "ИНН")` — correct. Available methods: `ОбщегоНазначения.ЗначениеРеквизитаОбъекта`, `ОбщегоНазначения.ЗначениеРеквизитовОбъекта`, `ОбщегоНазначения.ЗначениеРеквизитаОбъектов`
- Do not build user-facing strings with concatenation (`+`). Use `СтрШаблон()` with `НСтр()` and extract the result into a variable before passing it as an argument. This improves readability, localizability, and keeps function calls clean.
```bsl
// WRONG: concatenation inside a function call argument
Результат = НовыйРезультатВыполнения(
    Истина, "Записано для " + Количество + " пользователей" + ТекстОшибки);

// CORRECT: СтрШаблон + НСтр, extracted to a variable
ТекстРезультата = СтрШаблон(НСтр("ru = 'Записано для %1 пользователей %2'"), Количество, ТекстОшибки);
Результат = НовыйРезультатВыполнения(Истина, ТекстРезультата);
```
- Do not use ternary operators: `ИНН = ?(Контрагент.ИНН <> "", Контрагент.ИНН, "Не указан")`
- Do not use Hungarian notation for variables. Example: `МассивКонтрагентов` — incorrect, `Контрагенты` — correct.
- Do not use 1C global context names for variables (`Документы`, `Справочники`, `Пользователи`, `Регистры`, `Метаданные`, `Константы`, etc.). This creates name collisions and reduces readability.
- Do not name procedures, functions, or variables with names that match 1C built-in global context methods and functions (`Выполнить`, `ЗначениеЗаполнено`, `Тип`, `Строка`, `Число`, `Дата`, `Формат`, `Мин`, `Макс`, `Цел`, `Окр`, `Вычислить`, `ОписаниеОшибки`, `ПолучитьФорму`, `Подключить`, etc.). This shadows built-in functions, causes EDT errors, and produces unpredictable behavior. If a domain concept matches a built-in name, add a qualifying suffix — e.g. `ВыполнитьШаг` instead of `Выполнить`.
- Do not use `Попытка...Исключение` for database read/write operations without justified transaction management needs.
- Do not use `ЗаписьЖурналаРегистрации()` unless explicitly requested by the user.
- Do not compare boolean values to `Истина`/`Ложь` — use boolean expressions directly. Example: `Если Активен Тогда` instead of `Если Активен = Истина Тогда`.
- Use query parameters (`Запрос.УстановитьПараметр()`) instead of string concatenation to prevent injection and improve performance.
- Use `ПЕРВЫЕ N` when only a limited number of records is needed.
- Use `УстановитьПривилегированныйРежим(Истина)` sparingly and only when necessary. Always disable after the operation: `УстановитьПривилегированныйРежим(Ложь)`.
- Use `ЗаполнитьЗначенияСвойств()` for bulk property assignment.
- For lookups in large collections, use `Соответствие` (O(1)) instead of array iteration (O(n)).
- Document public procedures and functions: purpose, parameters, return value.
- Don't use letter "ё" or "Ё", use "е" or "Е" instead.
- Don't use letter "—"

### Formatting
- Do not break a line if only a single variable would remain on the new line.
- Add blank lines before and after code inside condition and loop blocks for readability.

### Metadata Conventions

- Metadata objects and attributes prefixed with `Удалить` (e.g. `УдалитьКонтрагент`, `УдалитьАдрес`) are deprecated stubs kept for backward compatibility. Treat them as non-existent: do not read, write, reference, or extend them in new code.

### Other Rules
- Use `common-rules.md` rules for common rules
- For queries, use `1c-query-optimization.md` rules
- Use `1c-ssl.md` rules for SSL (БСП) usage for functions and procedures that require it
- Use `1c-edt-metadata.md` rules for working with 1C metadata files in EDT format (`.mdo`, `.form`, and other XML files) — UUID generation, structure of attributes