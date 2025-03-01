---
sourcePath: ru/ydb/ydb-docs-core/ru/core/yql/reference/yql-core/builtins/_includes/aggregation/agg_list.md
sourcePath: ru/ydb/yql/reference/yql-core/builtins/_includes/aggregation/agg_list.md
---
## AGGREGATE_LIST {#agg-list}

Получить все значения столбца в виде списка. В сочетании с `DISTINCT` возвращает только уникальные значения. Опциональный второй параметр задает максимальное количество получаемых значений.  

Если заранее известно, что уникальных значений не много, то лучше воспользоваться агрегатной функцией `AGGREGATE_LIST_DISTINCT`, которая строит тот же результат в памяти (которой при большом числе уникальных значений может не хватить).

Порядок элементов в результирующем списке зависит от реализации и снаружи не задается. Чтобы получить упорядоченный список, необходимо отсортировать результат, например с помощью [ListSort](../../list.md#listsort).

Чтобы получить список нескольких значений с одной строки, важно *НЕ* использовать функцию `AGGREGATE_LIST` несколько раз, а сложить все нужные значения в контейнер, например через [AsList](../../basic.md#aslist) или [AsTuple](../../basic.md#astuple) и передать этот контейнер в один вызов `AGGREGATE_LIST`.

Например, можно использовать в сочетании с `DISTINCT` и функцией [String::JoinFromList](../../../udf/list/string.md) (аналог `','.join(list)` из Python) для распечатки в строку всех значений, которые встретились в столбце после применения [GROUP BY](../../../syntax/group_by.md).

**Примеры**

``` yql
SELECT  
   AGGREGATE_LIST( region ),
   AGGREGATE_LIST( region, 5 ),
   AGGREGATE_LIST( DISTINCT region ),
   AGGREGATE_LIST_DISTINCT( region ),
   AGGREGATE_LIST_DISTINCT( region, 5 )
FROM users
```

``` yql
-- Аналог GROUP_CONCAT из MySQL
SELECT
    String::JoinFromList(CAST(AGGREGATE_LIST(region, 2) AS List<String>), ",")
FROM users
```
Существует также короткая форма записи этих функций - `AGG_LIST` и `AGG_LIST_DISTINCT`.

{% note alert %}

Выполняется **НЕ** ленивым образом, поэтому при использовании нужно быть уверенным, что список получится разумных размеров, примерно в пределах тысячи элементов. Чтобы подстраховаться, можно воспользоваться вторым опциональным числовым аргументом, который включает ограничение на число элементов в списке.

{% endnote %}