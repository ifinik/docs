---
sourcePath: ru/ydb/ydb-docs-core/ru/core/yql/reference/yql-core/syntax/_includes/group_by/compact.md
sourcePath: ru/ydb/yql/reference/yql-core/syntax/_includes/group_by/compact.md
---
## GROUP COMPACT BY

Позволяет более эффективно выполнять агрегацию в тех случаях, когда автору запроса заранее известно, что ни по одному из ключей агрегации не встречаются большие объемы данных (больше примерно гигабайт или миллионов строк). Если это предположение на практике окажется неверным, то операция оставляет за собой право упасть из-за превышения потребления оперативной памяти или работать значительно медленнее не-COMPACT версии.

В отличие от обычного GROUP BY, отключается стадия Map-side combiner и дополнительные Reduce для каждого поля с [DISTINCT](#distinct) агрегацией.

**Пример:**
``` yql
SELECT
  key,
  COUNT(DISTINCT value) AS count -- топ-3 ключей по количеству уникальных значений
FROM my_table
GROUP COMPACT BY key
ORDER BY count DESC
LIMIT 3;
```
