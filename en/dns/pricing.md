---
editable: false
---
# Pricing for {{ dns-name }}

## Public DNS queries {#public-dns-requests}




{% include [usd-public-dns-requests.md](../_pricing/dns/usd-public-dns-requests.md) %}

You will be billed for:

* Authoritative queries: Any requests for DNS records in a user's [public zones](concepts/dns-zone.md#public-zones) from the internet or {{ yandex-cloud }}.

* Recursive queries: Queries from a {{ yandex-cloud }} VM for external domain names on the internet.

    {% note tip %}

    We recommend using [caching resolvers](tutorials/local-dns-cache.md), such as `systemd-resolved`, `dnsmasq`, or `unbound`. They can help you reduce the number of queries for external domain names, reducing costs.

    {% endnote %}

You will not be billed for:

* Queries to [private](concepts/dns-zone.md#private-zones) and [service](concepts/dns-zone.md#service-zones) zones.

**Starting December 6, 2021, the following is provided free of charge**:

* The first one million (1000000) recursive queries from VMs each month.

  Once you have used up this free amount, you will be charged at the published rate.

  The unused balance of free services is reset at the end of the month.

* Queries for domain names of {{ yandex-cloud }} services (such as `api.cloud.yandex.net`) and those offered by Yandex (such as `mail.yandex.ru`), both from VMs and the internet.

The cost of public DNS queries is based on usage.




> For example, if the price of 1 million authoritative queries is $0.256410, and 50000 queries were made during this month,
> the monthly cost will be:
> (50000 / 1000000) × $0.256410 = 0.05 × $0.256410 = $0.0128205

> For example, if the price of 1 million recursive queries is $0.256410, and 1.2 million recursive queries were made from your VMs
> between December 6, 2021 and December 31, 2021, the monthly cost will be:
> (1.2 − 1.0) × $0.256410 = 0.2 × 0.256410 = $0.0513
> because the first million recursive queries are provided free of charge as of December 6, 2021.

## DNS zones {#dns-zones}

You will be charged for any user-created [public](concepts/dns-zone.md#public-zones) and [private](concepts/dns-zone.md#private-zones) DNS zones.

[Service zones](concepts/dns-zone.md#service-zones) are provided free of charge.

Billing occurs per second. The cost is calculated in proportion to usage.

> For example, the cost of owning 2 DNS zones for 360 hours will be the same as the cost of owning 1 DNS zone
> for 720 hours.




{% include [usd-public-dns-zones.md](../_pricing/dns/usd-dns-zones.md) %}

