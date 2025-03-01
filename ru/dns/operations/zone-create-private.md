# Создать внутреннюю зону DNS

Чтобы создать внутреннюю [зону DNS](../concepts/dns-zone.md):

{% list tabs %}

- Консоль управления

  1. В [консоли управления]({{ link-console-main }}) выберите каталог, где требуется создать зону DNS.
  1. Выберите сервис **{{ dns-name }}**.
  1. Нажмите кнопку **Создать зону**.
  1. Задайте настройки зоны:
     1. **Зона** — доменная зона. Название зоны должно заканчиваться точкой.
     1. **Тип** — внутренняя. 
     1. Укажите сети, ресурсы из которых будут входить в создаваемую зону.
     1. **Имя** зоны.
  1. Нажмите кнопку **Создать**.

- CLI

  {% include [include](../../_includes/cli-install.md) %}

  {% include [default-catalogue](../../_includes/default-catalogue.md) %}

  Чтобы создать новую зону DNS:

  1. Посмотрите описание команды CLI для создания зоны:

     ```
     yc dns zone create --help
     ```

  1. Создайте новую внутреннюю зону DNS в каталоге по умолчанию:

     ```
     yc dns zone create --name test-zone \
     --zone staging. \
     --private-visibility network-ids=<идентификаторы сетей для зоны>
     ```

     Результат выполнения команды:

     ```
     id: aet29qhara5jeg45tbjg
     folder_id: aoerb349v3h4bupphtaf
     created_at: "2021-02-21T09:21:03.935Z"
     name: test-zone
     zone: staging.
     private_visibility:
       network_ids:
       - <идентификатор сети>
     ```


- Terraform

  Если у вас ещё нет Terraform, [установите его и настройте провайдер {{ yandex-cloud }}](../../tutorials/infrastructure-management/terraform-quickstart.md#install-terraform).

  1. Опишите в конфигурационном файле параметры ресурсов, которые необходимо создать:
     
     Параметры зоны DNS:

     * `zone` — доменная зона. Название зоны должно заканчиваться точкой. Публичные зоны верхнего уровня (TLD-зоны) создавать нельзя. Обязательный параметр.
     * `folder_id` — идентификатор каталога, в котором будет создана зона. Если он не указан, используется каталог по умолчанию. Необязательный параметр.
     * `name` — имя зоны. Должно быть уникальное внутри каталога. Необязательный параметр.
     * `description` — описание зоны. Необязательный параметр.
     * `labels` — набор меток зоны DNS. Необязательный параметр.
     * `public` — видимость зоны: публичная или внутренняя. Необязательный параметр.
     * `private_networks` — для внутренних зон указываются ресурсы {{ vpc-name }}, которым доступны доменные имена из этой зоны. Необязательный параметр.


     Параметры записи DNS:

     * `zone_id` — идентификатор зоны, в которой будет находиться этот набор записей. Обязательный параметр.
     * `name` — доменное имя. Обязательный параметр.
     * `type` — тип DNS записи. Обязательный параметр.
     * `ttl` — время жизни записи (TTL, Time to live) в секундах до актуализации информации о значении записи. Необязательный параметр.
     * `data` — значение записи. Необязательный параметр.


     ```hcl
     resource "yandex_vpc_network" "foo" {}
     
     resource "yandex_dns_zone" "zone1" {
       name        = "my-private-zone"
       description = "Test private zone"
     
       labels = {
         label1 = "test-private"
       }
     
       public           = false
       private_networks = [yandex_vpc_network.foo.id]
     }
     
     resource "yandex_dns_recordset" "rs1" {
       zone_id = yandex_dns_zone.zone1.id
       name    = "srv.example.com."
       type    = "A"
       ttl     = 200
       data    = ["10.1.0.1"]
     }
     ```
   


     Более подробную информацию о ресурсах, которые вы можете создать с помощью Terraform, смотрите в [документации провайдера](https://www.terraform.io/docs/providers/yandex/index.html).


  1. Выполните проверку с помощью команды:
     ```
     terraform plan
     ```
  
     В терминале будет выведен список ресурсов с параметрами. Это проверочный этап: ресурсы не будут созданы. Если в конфигурации есть ошибки, Terraform на них укажет.
  
       {% note alert %}
    
       Все созданные с помощью Terraform ресурсы тарифицируются. Внимательно проверьте план.
    
       {% endnote %}
  
  1. Чтобы создать ресурсы выполните команду:
     ```
     terraform apply
     ```
     
  1. Подтвердите создание ресурсов: введите в терминал слово `yes` и нажмите Enter.
  
     Terraform создаст все требуемые ресурсы. Проверить появление ресурсов можно в [консоли управления]({{ link-console-main }}) или с помощью команды [CLI](../../cli/quickstart.md):

     ```
     yc dns zone get <имя зоны DNS>
     ```

{% endlist %}
