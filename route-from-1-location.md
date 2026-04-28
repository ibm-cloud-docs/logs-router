---

copyright:
  years:  2023, 2026
lastupdated: "2026-04-28"

keywords:

subcollection: logs-router

---

{{site.data.keyword.attribute-definition-list}}



# Routing metrics from 1 location
{: #route-from-1-location}

To route metrics that are generated in 1 {{site.data.keyword.logs_routing_full_notm}} supported location to a destination target, you must create a route in your {{site.data.keyword.cloud_notm}} account that specifies the target and the location as an inclusion filter.
{: shortdesc}

## Prereqs
{: #route-from-1-location-prereqs}
{: cli}

1. [Install the {{site.data.keyword.cloud_notm}} CLI](/docs/cli?topic=cli-install-ibmcloud-cli).

2. [Install the {{site.data.keyword.logs_routing_full_notm}} CLI](/docs/logs-router?topic=logs-router-logs-router-cli-config).

3. Ensure you have the [correct IAM permissions to configure {{site.data.keyword.logs_routing_full_notm}} routes.](/docs/logs-router?topic=logs-router-iam)

4. Log in to {{site.data.keyword.cloud_notm}}. Run the following command: [ibmcloud login](/docs/cli?topic=cli-ibmcloud_cli#ibmcloud_login).

## Get the target details
{: #route-from-1-location-step1}
{: cli}
{: step}

Complete the following steps:

1. Run the following command to list all targets. Copy the target ID of the one where you want to route the metrics.

    ```text
    ibmcloud logs-router target ls
    ```
    {: pre}

2. Run the following command to get the target details:

    ```text
    ibmcloud logs-router target get --target <TARGET_ID>
    ```
    {: pre}


## Define the inclusion filter
{: #route-from-1-location-step2}
{: cli}
{: step}

Inclusion filters determine which metrics are routed to the targets.

Inclusion filters are comprised of an `operand`, `operator`, and `values`:

`operand`
:   Operand is the name of the property on which the `operator` test is run. The following operands are supported: `location`,`service_name`,`service_instance`,`resource_type`, and `resource`.

`operator`
:   Two operators are supported: `in` and `is`.

    `in`
    :   The value of the operand property is compared to a list of values.

        You can define up to 20 values.

    `is`
    :   The value of the operand property is compared to a single value.

        When using `is`, only 1 value can be specified.

`value`
:   A string, or an array of strings, to be compared with the `operand` property to determine whether the metric is routed or not. When the `is` `operator` is used, `value` must include a single string. When the `in` `operator` is used, `value` can include multiple strings in an array.

    Valid values depend on the `operand`.

    `location`
    :   Any location where [{{site.data.keyword.logs_routing_full_notm}} is available.](/docs/logs-router?topic=logs-router-regions)

    `service_name`
    :   The CRN service name of an [{{site.data.keyword.cloud_notm}} service that generates metrics managed through [{{site.data.keyword.logs_routing_full_notm}}](/docs/logs-router?topic=logs-router-cloud-services-mr)

    `service_instance`, `resource_type`, and `resource`
    :   Values appropriate to an [{{site.data.keyword.cloud_notm}} service that generates metrics managed through [{{site.data.keyword.logs_routing_full_notm}}](/docs/logs-router?topic=logs-router-cloud-services-mr)


For example, to define an inclusion filter that defines the condition where only metrics that are generated in the us-south region are routed, looks as follows:

```json
{"operand": "location","operator": "is","values": "us-south"}
```
{: codeblock}


## Configure the route
{: #route-from-1-location-step3}
{: cli}
{: step}

Run the following command to route metrics from us-south to the target identified in previous steps.

```text
ibmcloud logs-router route create --name new-route --rules '[{"action": "send", "targets":[{"id":"11111111-1111-1111-1111-1111111111111"}], "inclusion_filters":[{"operand": "location","operator": "is","values": "us-south"}]}]'
```
{: pre}

Where `targets` represents the list of target IDs and `inclusion_filters` specifies the filters.
