# Managing Apache Kafka® cluster hosts

You can get a list of broker hosts in your {{ KF }} cluster.

## Getting a list of cluster hosts {#list-hosts}

{% list tabs %}

- Management console

  1. Go to the folder page and select **{{ mkf-name }}**.
  1. Click on the name of the cluster you need and select the **Hosts** tab.

- CLI

  {% include [cli-install](../../_includes/cli-install.md) %}

  {% include [default-catalogue](../../_includes/default-catalogue.md) %}

  To get a list of cluster hosts, run the command:

  ```bash
  {{ yc-mdb-kf }} cluster list-hosts <cluster name>
  ```

  You can query the cluster ID and name with a [list of clusters in the folder](cluster-list.md#list-clusters).

- API

  Use the [listHosts](../api-ref/Cluster/listHosts.md) API method: pass the ID of the required cluster in the `clusterId` request parameter.

  To find out the cluster ID, [get a list of clusters in the folder](cluster-list.md#list-clusters).

{% endlist %}

