---
sourcePath: ru/ydb/ydb-docs-core/ru/core/getting_started/_includes/cli/ls_example_generic.md
---
Например, если:

* Эндпоинт: `grpc://ydb.example.com:2136`;
* Имя базы данных: `/john/db1`;
* База данных не требует аутентификации, или задана нужная переменная окружения, как описано [здесь](../../auth.md);
* База данных только что создана и не содержит объектов;

то команда будет выглядеть следующим образом:

```bash
{{ ydb-cli }} -e grpc://ydb.example.com:2136 -d /john/db1 scheme ls
```

Результат выполнения на только что созданной пустой базе данных:

```text
.sys_health .sys
```
