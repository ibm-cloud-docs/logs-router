---

copyright:
  years:  2023, 2026
lastupdated: "2026-04-28"

keywords:

subcollection: logs-router

---

{{site.data.keyword.attribute-definition-list}}



# Excluding metrics by using the drop action
{: #route-drop}

You can configure {{site.data.keyword.logs_routing_full_notm}} to exclude (drop) metrics based on a configured rule. Dropped metrics are not sent on to a target.
{: shortdesc}

## Prereqs
{: #route-drop-prereqs}
{: cli}

1. [Install the {{site.data.keyword.cloud_notm}} CLI](/docs/cli?topic=cli-install-ibmcloud-cli).

2. [Install the {{site.data.keyword.logs_routing_full_notm}} CLI](/docs/logs-router?topic=logs-router-logs-router-cli-config).

3. Ensure you have the [correct IAM permissions to configure {{site.data.keyword.logs_routing_full_notm}} routes.](/docs/logs-router?topic=logs-router-iam)

4. Log in to {{site.data.keyword.cloud_notm}}. Run the following command: [ibmcloud login](/docs/cli?topic=cli-ibmcloud_cli#ibmcloud_login).


## Define the inclusion filter
{: #route-drop-step1}
{: cli}

Inclusion filters determine which metrics are routed to the targets.

Inclusion filters are comprised of an `operand`, `operator`, and `values`:

`operand`
:   Operand is the name of the property on which the `operator` test is run. The following operand is supported: `location`.

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
    :   Any location where [{{site.data.keyword.logs_routing_full_notm}} is available.](/docs/logs-router?topic=logs-router-locations)


For example, to define an inclusion filter that defines the condition where only metrics that are generated in the us-south region are routed, looks as follows:

```json
{"operand": "location","operator": "is","values": "us-south"}
```
{: codeblock}


## Configure the route
{: #route-drop-step2}
{: cli}

Run the following command to exclude all metrics received by {{site.data.keyword.logs_routing_full_notm}} from the `us-south` region.

```text
ibmcloud logs-router route create --name drop-route --rules '[{"action": "drop", "inclusion_filters":[{"operand": "location","operator": "is","values": "us-south"}]}]'
```
{: pre}

Where `inclusion_filters` specifies the filters.
