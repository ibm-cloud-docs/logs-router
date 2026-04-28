---

copyright:
  years:  2023, 2026
lastupdated: "2026-04-28"

keywords:

subcollection: logs-router

---

{{site.data.keyword.attribute-definition-list}}


# Defining routing rules
{: #route_rules_definitions}

To define a routing rule, you must specify 1 or more targets as the destinations for metrics. You can also define 1 or more inclusion filters that define the conditions of how those metrics are routed to those destinations.
{: shortdesc}

For each route that you define in the account, you can configure up to 10 rules. The rules specify what metrics are routed in a region and where to route them. For more information, see [Understanding how routes work in your account](/docs/logs-router?topic=logs-router-routes&interface=cli#route_behaviour).

A rule consists of 1 action, 1 or more targets, and 0 or more inclusion filters.



## Targets
{: #route_rules_definitions_targets}

Targets define the list of target IDs where the metrics are routed.

- You can specify up to three target IDs per rule.

- You can define target IDs for resources that are available in the same region where you are configuring the route, in a different region, and in a different account.

For example, you can define a list of targets as follows:

```text
"targets": [{"id":"11111111-1111-1111-1111-111111111111"},{"id":"22222222-2222-2222-2222-222222222222"}]
```
{: codeblock}

Targets must be {{site.data.keyword.mon_full_notm}} instances.
{: restriction}


## Action
{: #route_rules_definitions_action}

Action defines whether {{site.data.keyword.logs_routing_full_notm}} includes or excludes metrics on the route. Two actions are supported: `send` and `drop`. If not specified, the default action is to send the metrics.

`send`
:   Metrics are sent, based on the routing rule, on the defined route.

`drop`
:   Metrics are excluded, based on the routing rule, when metrics are sent on the defined route.

## Inclusion filters
{: #route_rules_definitions_filters}

Inclusion filters define the conditions that are used to determine which metrics are routed to the targets specified in the rule.

To route all metrics, exclude the `inclusion_filters` definition when you configure a route.
{: note}

Inclusion filters are composed of an `operand`, `operator`, and `value`:

`operand`
:   Operand is the name of the property in the target that is used to filter data. The following operands are supported: `location`, `service_name`, `service_instance`, `resource_type`, and `resource`. The value is extracted from the target CRN.

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

Note these limitations when configuring inclusion filters.

* You can configure up to 7 inclusion filters for each rule.

* You can configure up to 20 values for `inclusion_filter.values`.

* Each value configured for `inclusion_filter.values` can be a maximum of 100 characters.

## IAM Access
{: #rules_iam}

Users must have the appropriate IAM roles to work with routing rules. For information on IAM roles, see [Managing IAM access.](/docs/logs-router?topic=logs-router-iam)

<ui>

## Defining routing rules by using the UI
{: #define_rule_ui}
{: ui}

You can configure action and inclusion filter routing rules in the UI. for more information, see [Managing routes](/docs/logs-router?topic=logs-router-route-manage&interface=ui).

</ui>

## Defining routing rules by using the CLI
{: #define_rule}
{: cli}

Rules for routes and inclusion filters are defined in JSON format.  Rules are defined in the following ways:

* By using the `--rules` option information is passed as JSON directly on the command.
* By using the `--files` option information is passed as a JSON file.


### Example that uses the --rules option
{: #define_rule_1}

The following example specifies rules directly on the `--rules` option. In this example, all metrics from `us-east` are routed to `11111111-1111-1111-1111-1111111111111`. All metrics from `appconnect`, `cloudant`, and `containers-kupernetes` are routed to `22222222-2222-2222-2222-222222222222` and `33333333-3333-3333-3333-3333333333333`.

```text
[{"action": "send", "targets":[{"id":"11111111-1111-1111-1111-1111111111111"}], "inclusion_filters":[{"operand": "location","operator": "is","values": ["us-east"]}]},{"targets":[{"id":"22222222-2222-2222-2222-222222222222"},{"id":"33333333-3333-3333-3333-3333333333333"}], "inclusion_filters":[{"operand": "service_name","operator": "in","values": ["appconnect","cloudant","containers-kubernetes"]}]}]'
```
{: codeblock}

### Example that uses the --file option
{: #define_rule_2}

The following example specifies rules by passing a JSON file that is named `rules_def.json` on the `--file` option:

```text
--file rules_def.json
```
{: codeblock}

The `rules_def.json` file contains:

```json
[
  {
    "action":"send",
    "targets": [
      {
        "id":"11111111-1111-1111-1111-111111111111"
      },
      {
        "id":"22222222-2222-2222-2222-222222222222"
      }
    ],
    "inclusion_filters": [
      {
          "operand": "service_name",
          "operator": "in",
          "values": [
            "appconnect",
            "cloudant",
            "containers-kubernetes"
          ]
        },
        {
          "operand": "location",
          "operator": "in",
          "values": [
            "us-south",
            "eu-de"
          ]
        }
    ]
  }
]
```
{: codeblock}

In this example, only metrics from `appconnect`, `cloudant`, and `containers-kubernetes` from the `us-south` and `eu-de` regions are routed to targets `11111111-1111-1111-1111-111111111111` and `22222222-2222-2222-2222-222222222222`.



## Define routing rules by using the API
{: #define_rules_api}
{: api}

Targets and inclusion filters are defined in API calls by using `rules`. The `rules` are defined in a JSON structure.

For example, the following example creates a route that is named `my-route` and sends metrics from `codeengine`, and `container-registry` in the `us-east` region to targets `11111111-1111-1111-1111-111111111111` and `22222222-2222-2222-2222-222222222222`.

```sh
curl -X POST https://private.<REGION>.logs-router.cloud.ibm.com/api/v3/routes -H "Authorization: Bearer <IAM_TOKEN>" -H 'content-type: application/json' -d '{
    "name": "my-route",
    "rules": [
      {
        "action":"send",
        "targets": [{"id":"11111111-1111-1111-1111-111111111111"}, {"id":"22222222-2222-2222-2222-222222222222"}],
        "inclusion_filters": [
          {
            "operand": "location",
            "operator": "is",
            "values": ["us-east"]
          },
          {
            "operand": "service_name",
            "operator": "in",
            "values": ["codeengine","container-registry"]
          }
        ]
      }
    ]
  }'
```
{: codeblock}


For more information, see the [API reference.](https://{DomainName}/apidocs/logs-router/logs-router-v3)
