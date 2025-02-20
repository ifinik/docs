# Invalid top-level LOD dimension found in expression

`ERR.DS_API.FORMULA.VALIDATION.LOD.INVALID_TOPLEVEL_DIMENSIONS`

Ошибка возникает из-за того, что агрегация верхнего уровня содержит измерения, которые не используются в чарте.

Например, в чарте с группировкой по измерениям `[Region]` и `[Category]` нельзя создать такой показатель:

```
AVG([Sales] INCLUDE [City])
```

Ошибка в этом случае возникает из-за того, что измерение `[City]` (добавленное в группировку с помощью `INCLUDE`) не используется в чарте.

Для исправления ошибки измените выражение так, чтобы в агрегации верхнего уровня содержались только измерения, использующиеся в чарте. Например, так:

```
AVG(AVG([Sales] INCLUDE [City]))
```

Тогда на верхнем уровне агрегация будет вычисляться с группировкой по измерениям чарта `[Region]` и `[Category]`, а вложенная агрегация — по измерениям `[Region]`, `[Category]` и `[City]`.

Подробнее об использовании LOD-выражений читайте в разделе [{#T}](../../concepts/lod-aggregation.md).
