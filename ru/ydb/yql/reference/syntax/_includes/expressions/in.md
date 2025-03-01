---
sourcePath: ru/ydb/ydb-docs-core/ru/core/yql/reference/yql-core/syntax/_includes/expressions/in.md
sourcePath: ru/ydb/yql/reference/yql-core/syntax/_includes/expressions/in.md
---
## IN {#in}
Проверка вхождения одного значения в набор значений. Логически эквивалентно цепочке сравнений на равенство через `OR`, но реализовано более эффективно.

{% note warning "Внимание" %}

В отличие от аналогичного ключевого слова в Python, в YQL `IN` **НЕ** является поиском подстроки в строке. Для поиска подстроки можно использовать функцию [String::Contains](../../../udf/list/string.md) или описанные выше [LIKE / REGEXP](#like).

{% endnote %}

Сразу после `IN` можно указать хинт `COMPACT`.
Если `COMPACT` не указан, то `IN` с подзапросом по возможности выполняется как соответствующий `JOIN` (`LEFT SEMI` для `IN` и `LEFT ONLY` для `NOT IN`).
Наличие хинта `COMPACT` форсирует in-memory стратегию выполнения: из содержимого правой части `IN` в памяти сразу строится хеш-таблица, по которой затем фильтруется левая часть.

Хинтом `COMPACT` следует пользоваться с осторожностью. Поскольку хеш-таблица строится в памяти, то запрос может упасть, если правая часть `IN` содержит много больших и/или различных элементов.


**Примеры**

``` yql
SELECT column IN (1, 2, 3)
FROM my_table;
```

``` yql
SELECT * FROM my_table
WHERE string_column IN ("a", "b", "c");
```

``` yql
$foo = AsList(1, 2, 3);
SELECT 1 IN $foo;
```

``` yql
$values = (SELECT column + 1 FROM table);
SELECT * FROM my_table WHERE
    -- фильтрация по in-memory хеш-таблице на основе table
    column1 IN COMPACT $values AND
    -- с последующим LEFT ONLY JOIN с other_table
    column2 NOT IN (SELECT other_column FROM other_table);
```
