---
sourcePath: ru/ydb/ydb-docs-core/ru/core/reference/ydb-cli/_includes/auth/options_cloud_additional.md
---
При использовании режимов аутентификации, предусматривающих ротацию токена с его периодическим перезапросом в IAM (**Refresh Token**, **Service Account Key**), может быть установлен специальный параметр, указывающий где размещен сервис IAM:

- `--iam-endpoint <URL>` : Задает URL сервиса IAM для обращения за новыми токенами в режимах аутентификации, подразумевающих ротацию токенов. Значение по умолчанию — `"iam.api.cloud.yandex.net"`.


