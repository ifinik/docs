# Миграция базы данных в {{ mmy-name }}

Перенести данные из стороннего _кластера-источника_ в _кластер-приемник_ {{ mmy-name }} можно двумя способами:

* [{#T}](#data-transfer).

    Этот способ прост в настройке, не требует создания промежуточной виртуальной машины и позволяет перенести базу целиком без остановки обслуживания пользователей. Для его использования необходимо разрешить подключение к кластеру-источнику из интернета.
    Подробнее см. в разделе [{#T}](../../data-transfer/concepts/use-cases.md).

* [{#T}](#logical-dump).

    _Логический дамп_ — файл с набором команд, последовательное выполнение которых позволяет восстановить состояние базы данных. Чтобы обеспечить полноту логического дампа, перед его созданием кластер-источник следует перевести в режим <q>только чтение</q>.

    Этот способ используйте только в том случае, если перенос данных с помощью {{ data-transfer-full-name }} по каким-либо причинам невозможен.

## Перед началом работы {#before-you-begin}

 [Создайте кластер {{ mmy-name }}](../operations/cluster-create.md) любой подходящей конфигурации. При этом:

* Версия {{ MY }} должна быть не ниже чем на кластере-источнике.

    Перенос данных c повышением мажорной версии {{ MY }} возможен, но не гарантируется. Подробнее см. в [документации {{ MY }}](https://dev.mysql.com/doc/refman/8.0/en/faqs-migration.html).

    Миграция с понижением версии {{ MY }} [невозможна](https://dev.mysql.com/doc/refman/8.0/en/downgrading.html).

* [Режим SQL](../concepts/settings-list.md#setting-sql-mode) должен быть таким же, как и на кластере-источнике.

## Перенос данных с использованием сервиса {{ data-transfer-full-name }} {#data-transfer}

{% include notitle [Migration with Data Trasnfer](../../_includes/tutorials/datatransfer/managed-mysql.md) %}

## Перенос данных через создание и восстановление логического дампа {#logical-dump}

Чтобы перенести данные в кластер {{ mmy-name }}, создайте логический дамп нужной базы и восстановите его в кластере-приемнике. Это можно сделать двумя способами:

* С помощью [утилит `mydumper` и `myloader`](https://github.com/mydumper/mydumper). Дамп базы создается в виде набора файлов в отдельном каталоге.
* С помощью утилит `mysqldump` и `mysql`. Дамп базы создается в виде одного файла.

Этапы миграции:

1. [Создайте дамп](#dump) переносимой базы.
1. При необходимости [создайте промежуточную виртуальную машину](#create-vm) в {{ yandex-cloud }} и загрузите дамп на нее.
1. [Восстановите данные из дампа](#restore).


Если созданные ресурсы вам больше не нужны, [удалите их](#clear-out).

### Создание дампа {#dump}

{% list tabs %}

* С использованием утилиты mysqldump

    1. Переключите базу в режим <q>только чтение</q>, чтобы не потерять данные, которые бы появились во время создания дампа.

    1. Установите утилиту `mysqldump` на кластер-источник, например (для Ubuntu):

        ```bash
        sudo apt update && sudo apt install mysql-client --yes
        ```

    1. Создайте дамп базы данных:

        ```bash
        mysqldump \
            --host=<FQDN или IP-адрес хоста-мастера в кластере-источнике> \
            --user=<имя пользователя> \
            --password \
            --port=<порт> \
            --set-gtid-purged=OFF \
            --quick \
            --single-transaction \
            <имя базы данных> > ~/db_dump.sql
        ```

        При необходимости передайте в команде создания дампа дополнительные параметры:

        * `--events` — если в вашей базе есть периодические события;
        * `--routines` — если в вашей базе есть хранимые процедуры и функции.

        Для таблиц InnoDB используйте опцию `--single-transaction`: она гарантирует целостность данных.

    1. В файле дампа исправьте имена движков таблиц на `InnoDB`:

        ```bash
        sed -i -e 's/MyISAM/InnoDB/g' -e 's/MEMORY/InnoDB/g' db_dump.sql
        ```

    1. Упакуйте дамп в архив:

        ```bash
        tar -cvzf db_dump.tar.gz ~/db_dump.sql
        ```

* С использованием утилиты mydumper

    1. Переключите базу в режим <q>только чтение</q>, чтобы не потерять данные, которые бы появились во время создания дампа.

    1. Создайте директорию для файлов дампа:

        ```bash
        mkdir db_dump
        ```

    1. Установите утилиту `mydumper` на кластер-источник, например (для Ubuntu):

        ```bash
        sudo apt update && sudo apt install mydumper --yes
        ```

    1. Создайте дамп базы данных:

        ```bash
        mydumper \
            --triggers \
            --events \
            --routines \
            --outputdir=db_dump \
            --rows=10000000 \
            --threads=8 \
            --compress \
            --database=<имя базы данных> \
            --user=<имя пользователя> \
            --ask-password \
            --host=<FDQN или IP-адрес хоста-мастера в кластере-источнике>
        ```

        Где:

        * `--triggers` — дамп триггеров.
        * `--events` — дамп событий.
        * `--routines` — дамп хранимых процедур и функций.
        * `--outputdir` — директория для файлов дампа.
        * `--rows` — количество строк во фрагментах, на которые будут разбиты таблицы. Чем меньше значение, тем больше будет файлов в дампе.
        * `--threads` — количество используемых потоков. Рекомендуется использовать значение, равное половине свободных ядер на сервере.
        * `--compress` — сжатие выходных файлов.

    1. В файлах дампа исправьте имена движков таблиц на `InnoDB`:

        ```bash
        sed -i -e 's/MyISAM/InnoDB/g' -e 's/MEMORY/InnoDB/g' `find /db_dump -name '*-schema.sql'`
        ```

    1. Упакуйте дамп в архив:

        ```bash
        tar -cvzf db_dump.tar.gz ~/db_dump
        ```

{% endlist %}

### (опционально) Создание виртуальной машины в {{ yandex-cloud }} и загрузка дампа {#create-vm}

Переносить данные на промежуточную виртуальную машину в {{ compute-full-name }} нужно, если:

* К вашему кластеру {{ mmy-name }} нет доступа из интернета.
* Ваше оборудование или соединение с кластером в {{ yandex-cloud }} недостаточно надежны.

Нужное количество оперативной памяти, ядер процессора и дискового пространства зависит от объема переносимых данных и требуемой скорости переноса.

Чтобы подготовить виртуальную машину для восстановления дампа:

1. [Создайте виртуальную машину](../../compute/operations/vm-create/create-linux-vm.md) на базе [Ubuntu Linux 20.04](https://cloud.yandex.ru/marketplace/products/f2eanb2gaki4us67hn9q) со следующими параметрами:

    * **Диски и файловые хранилища** → **Размер** — достаточный для хранения распакованного и нераспакованого дампов.

        Рекомендуется использовать объем в два или более раза превышающий суммарный объем дампа и архива с ним.

    * **Сетевые настройки**:

        * **Подсеть** — выберите подсеть в той же облачной сети, в которой размещен кластер-приемник.
        * **Публичный адрес** — `Автоматически` или выберите из списка зарезервированных IP-адресов.

1. [Настройте группы безопасности](../operations/connect.md#configure-security-groups) промежуточной виртуальной машины и кластера {{ mmy-name }}.

1. [Подключитесь к промежуточной виртуальной машине по SSH](../../compute/operations/vm-connect/ssh.md).

1. Скопируйте архив с дампом базы данных на промежуточную виртуальную машину, например, используя утилиту `scp`:

    ```bash
    scp ~/db_dump.tar.gz <имя пользователя ВМ>@<публичный IP-адрес ВМ>:~/db_dump.tar.gz
    ```

1. Извлеките дамп из архива:

    ```bash
    tar -xzf ~/db_dump.tar.gz
    ```

### Восстановление данных {#restore}

{% note alert %}

Для кластера {{ mmy-name }} по умолчанию включен [AUTOCOMMIT](https://dev.mysql.com/doc/refman/8.0/en/server-system-variables.html#sysvar_autocommit). **Не отключайте** AUTOCOMMIT в рамках клиентской сессии при восстановлении базы данных из дампа, иначе возможно переполнение хранилища хоста и нарушение работы кластера.

{% endnote %}

{% list tabs %}

* С использованием утилиты mysql

    Этот способ подходит, если вы создали дамп с помощью утилиты `mysqldump`.

    1. Установите утилиту `mysql` на хост, с которого выполняется восстановление дампа, например (для Ubuntu):

        ```bash
        sudo apt update && sudo apt install mysql-client --yes
        ```

    1. Запустите восстановление базы из дампа:

        * Если вы восстанавливаете дамп с виртуальной машины в {{ yandex-cloud }}:

            ```bash
            mysql \
                --host=c-<идентификатор кластера-приемника>.rw.{{ dns-zone }} \
                --user=<имя пользователя> \
                --port={{ port-mmy }} \
                <имя базы данных> < ~/db_dump.sql
            ```

        * Если вы восстанавливаете дамп с хоста, подключающегося к {{ yandex-cloud }} из интернета, [получите SSL-сертификат](../operations/connect.md#get-ssl-cert) и передайте параметры `--ssl-ca` и `--ssl-mode` в команде восстановления:

            ```bash
            mysql \
                --host=c-<идентификатор кластера-приемника>.rw.{{ dns-zone }} \
                --user=<имя пользователя> \
                --port={{ port-mmy }} \
                --ssl-ca=~/.mysql/root.crt \
                --ssl-mode=VERIFY_IDENTITY \
                <имя базы данных> < ~/db_dump.sql
            ```

* С использованием утилиты myloader

    Этот способ подходит, если вы создали дамп с помощью утилиты `mydumper` и используете для восстановления промежуточную виртуальную машину.

    1. Установите утилиту `myloader` на хост, с которого выполняется восстановление дампа, например (для Ubuntu):

        ```bash
        sudo apt update && sudo apt install mydumper --yes
        ```

    1. Запустите восстановление базы из дампа:

        ```bash
        myloader \
            --host=c-<идентификатор кластера-приемника>.rw.{{ dns-zone }} \
            --directory=db_dump/ \
            --overwrite-tables \
            --threads=8 \
            --compress-protocol \
            --user=<имя пользователя> \
            --ask-password
        ```

{% endlist %}

Идентификатор кластера можно получить со [списком кластеров в каталоге](../operations/cluster-list.md#list-clusters).


### Удаление созданных ресурсов {#clear-out}

Если созданные ресурсы вам больше не нужны, удалите их:

1. Если вы создавали промежуточную виртуальную машину, [удалите ее](../../compute/operations/vm-control/vm-delete.md).
1. Если для промежуточной виртуальной машины был зарезервирован публичный статический IP-адрес, освободите и [удалите его](../../vpc/operations/address-delete.md).