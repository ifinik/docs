---
sourcePath: ru/ydb/ydb-docs-core/ru/core/reference/ydb-cli/export_import/_includes/tools_dump.md
---
# Выгрузка в файловую систему

Команда `tools dump` выгружает в клиентскую файловую систему данные и информацию об объектах схемы данных, в описанном в статье [Файловая структура](../file_structure.md) формате:

```bash
{{ ydb-cli }} [connection options] tools dump [options]
```

{% include [conn_options_ref.md](../../commands/_includes/conn_options_ref.md) %}

`[options]` - параметры команды:

`-p PATH` или `--path PATH`: Путь к директории в базе данных, объекты внутри которой должны быть выгружены, либо путь к таблице. По умолчанию -- корневая директория базы данных. В выгрузку будут включены все поддиректории, имена которых не начинаются с точки, и таблицы внутри них, имена которых не начинаются с точки. Для выгрузки таких таблиц или содержимого таких директорий можно явно указать их имена в данном параметре.

`-o PATH` или `--output PATH`: Путь к директории в клиентской файловой системе, куда должна быть сделана выгрузка. Если указанная директория не существует, то она будет создана, однако весь путь до неё должен существовать. Если указанная директория существует, то она должна быть пуста. Если параметр не указан, то в текущей директории будет сделана директория с именем в формате `backup_YYYYDDMMTHHMMSS`, где YYYYDDMM - дата, а HHMMSS - время начала выгрузки.

`--exclude STRING`: Шаблон ([PCRE](https://www.pcre.org/original/doc/html/pcrepattern.html)) для исключения путей из выгрузки. Данный параметр может быть указан несколько раз, для разных шаблонов.

`--scheme-only`: Выгружать только информацию об объектах схемы данных, без выгрузки данных

`--consistency-level VAL`: Уровень консистентности. Возможные варианты:
- `database` - Полностью консистентная выгрузка, со снятием одного снепшота перед началом выгрузки. Применяется по умолчанию.
- `table` - Консистентность в пределах каждой выгружаемой таблицы, со снятием отдельных независимых снепшотов для каждой выгружаемой таблицы. Может работать быстрее и оказывать меньшее влияние на обработку текущей нагрузки на базу данных.

`--avoid-copy`: Не применять создание снепшота для выгрузки. Применяемый по умолчанию для обеспечения консистентности снепшот может быть неприменим для некоторых случаев (например, для таблиц со внешними блобами).

`--save-partial-result`: Не удалять результат частично выполненной выгрузки. Без включения данной опции результат выгрузки будет удален, если в процессе её выполнения произойдет ошибка.

## Примеры

{% include [example_db1.md](../../_includes/example_db1.md) %}

### Выгрузка базы данных

С автоматическим созданием директории `backup_...` в текущей директории:

```
{{ ydb-cli }} --profile db1 tools dump 
```

В заданную директорию:

```
{{ ydb-cli }} --profile db1 tools dump -o ~/backup_db1
```

### Выгрузка структуры таблиц в заданной директории базы данных и её поддиректориях

```
{{ ydb-cli }} --profile db1 tools dump -p dir1 --scheme-only
```


