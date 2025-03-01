---
sourcePath: ru/ydb/ydb-docs-core/ru/core/yql/reference/yql-core/syntax/_includes/action/define_do.md
sourcePath: ru/ydb/yql/reference/yql-core/syntax/_includes/action/define_do.md
---
## DEFINE ACTION {#define-action}

Задает именованное действие, которое представляют собой параметризуемый блок из нескольких выражений верхнего уровня.

**Синтаксис**

1. `DEFINE ACTION` — объявление действия.
1. [Имя действия](../../expressions.md#named-nodes), по которому объявляемое действие доступно далее для вызова.
1. В круглых скобках — список имен параметров.
1. Ключевое слово `AS`.
1. Список выражений верхнего уровня.
1. `END DEFINE` — маркер последнего выражения внутри действия.

Один или более последних параметров могут быть помечены знаком вопроса `?` как необязательные. Если они не будут указаны при вызове, то им будет присвоено значение `NULL`.

## DO {#do}

Выполняет `ACTION` с указанными параметрами.

**Синтаксис**
1. `DO` — выполнение действия.
1. Именованное выражение, по которому объявлено действие.
1. В круглых скобках — список значений для использования в роли параметров.

`EMPTY_ACTION` — действие, которое ничего не выполняет.


**Пример**

```yql
DEFINE ACTION $hello_world($name, $suffix?) AS
    $name = $name ?? ($suffix ?? "world");
    SELECT "Hello, " || $name || "!";
END DEFINE;

DO EMPTY_ACTION();
DO $hello_world(NULL);
DO $hello_world("John");
DO $hello_world(NULL, "Earth");
```

