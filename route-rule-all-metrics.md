----

copyright:
  years:  2023, 2026
lastupdated: "2026-04-28"

keywords:

subcollection: logs-router

---

{{site.data.keyword.attribute-definition-list}}


# Routing all metrics
{: #route-rule-all-metrics}

Route all metrics that are generated in all of the {{site.data.keyword.logs_routing_full_notm}} supported locations to multiple destination targets.
{: shortdesc}

A rule consists of 1 or more targets, and 1 or more inclusion filters. For more information on how routes work, see [Understanding how routes work in your account](/docs/metrics-router?topic=metrics-router-routes&interface=cli#route_behaviour).

Inclusion filters determine which metrics are routed to the targets.

To route all metrics, you must exclude the `inclusion_filters` definition when you configure the route.
{: note}


## Prereqs
{: #route-rule-all-metrics-prereqs}
{: cli}

1. [Install the {{site.data.keyword.cloud_notm}} CLI](/docs/cli?topic=cli-install-ibmcloud-cli).

2. [Install the {{site.data.keyword.logs_routing_full_notm}} CLI](/docs/metrics-router?topic=metrics-router-metrics-router-cli-config).

3. Ensure you have the [correct IAM permissions to configure {{site.data.keyword.logs_routing_full_notm}} routes.](/docs/metrics-router?topic=metrics-router-iam)

4. Log in to {{site.data.keyword.cloud_notm}}. Run the following command: [ibmcloud login](/docs/cli?topic=cli-ibmcloud_cli#ibmcloud_login).

## Get the target details
{: #route-rule-all-metrics-step1}
{: cli}
{: step}

Complete the following steps:

1. Run the following command to list all targets. Copy the target ID of the one where you want to route the metrics.

    ```text
    ibmcloud metrics-router target ls
    ```
    {: pre}

2. Run the following command to get the details of the targets where you want to route the metrics:

    ```text
    ibmcloud metrics-router target get --target <TARGET_ID>
    ```
    {: pre}



## Configure the route
{: #route-rule-all-metrics-step2}
{: cli}
{: step}

Run the following command to send all metrics from supported locations in your account to the targets identified in previous steps.

```text
ibmcloud metrics-router route create --name route-all --rules '[{"action": "send", "targets":[{"id":"11111111-1111-1111-1111-111111111111"},{"id":"22222222-2222-2222-2222-222222222222"}]}]'
```
{: pre}
