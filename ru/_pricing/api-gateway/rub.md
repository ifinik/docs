### Запросы к API-шлюзам {#request}

| Услуга | Цена за 1 млн запросов, вкл. НДС |
| --- | --- |
| Запросы к API-шлюзам, до 100 000 запросов в месяц | {{ sku|RUB|api-gateway.requests.v1|string }} |
| Запросы к API-шлюзам, свыше 100 000 запросов в месяц | {{ sku|RUB|api-gateway.requests.v1|pricingRate.0.1|string }} |

Оплачивается фактическое количество вызовов.

>Например, стоимость десяти тысяч вызовов составит `1,2 ₽`, при условии стоимости 1 млн запросов `120 ₽`.