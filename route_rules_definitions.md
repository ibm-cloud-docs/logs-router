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

To define a routing rule, you must specify 1 or more targets as the destinations for platform logs. You can also define 1 or more inclusion filters that define the conditions of how those platform logs are routed to those destinations.
{: shortdesc}

For each route that you define in the account, you can configure up to 10 rules. The rules specify what platform logs are routed in a region and where to route them. For more information, see [Understanding how routes work in your account](/docs/logs-router?topic=logs-router-routes&interface=cli#route_behaviour).

A rule consists of 1 action, 1 or more targets, and 0 or more inclusion filters.



## Targets
{: #route_rules_definitions_targets}

Targets define the list of target IDs where the platform logs are routed.

- You can specify up to three target IDs per rule.

- You can define target IDs for resources that are available in the same region where you are configuring the route, in a different region, and in a different account.

For example, you can define a list of targets as follows:

```text
"targets": [{"id":"11111111-1111-1111-1111-111111111111"},{"id":"22222222-2222-2222-2222-222222222222"}]
```
{: codeblock}

Targets must be {{site.data.keyword.logs_full_notm}} instances.
{: restriction}


## Action
{: #route_rules_definitions_action}

Action defines whether {{site.data.keyword.logs_routing_full_notm}} includes or excludes platform logs on the route. Two actions are supported: `send` and `drop`. If not specified, the default action is to send the platform logs.

`send`
:   Platform logs are sent, based on the routing rule, on the defined route.

`drop`
:   Platform logs are excluded, based on the routing rule, when platform logs are sent on the defined route.

## Inclusion filters
{: #route_rules_definitions_filters}

Inclusion filters define the conditions that are used to determine which platform logs are routed to the targets specified in the rule.

To route all platform logs, exclude the `inclusion_filters` definition when you configure a route.
{: note}

Inclusion filters are composed of an `operand`, `operator`, and `value`:

`operand`
:   Operand is the name of the property in the target that is used to filter data. The following operands is supported: `location`. The value is extracted from the target CRN.

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

Note these limitations when configuring inclusion filters.

* You can configure up to 7 inclusion filters for each rule.

* You can configure up to 20 values for `inclusion_filter.values`.

* Each value configured for `inclusion_filter.values` can be a maximum of 100 characters.

## IAM Access
{: #rules_iam}

Users must have the appropriate IAM roles to work with routing rules. For information on IAM roles, see [Managing IAM access.](/docs/logs-router?topic=logs-router-iam)



## Defining routing rules by using the UI
{: #define_rule_ui}
{: ui}

You can configure action and inclusion filter routing rules in the UI. For more information, see [Managing routes](/docs/logs-router?topic=logs-router-route-manage&interface=ui).


## Defining routing rules by using the CLI
{: #define_rule}
{: cli}

Rules for routes and inclusion filters are defined in JSON format. Rules are defined in the following ways:

* By using the `--rules` option, information is passed as JSON directly on the command.
* By using the `--files` option, information is passed as a JSON file.


### Example that uses the --rules option
{: #define_rule_1}

The following example specifies rules directly on the `--rules` option. In this example, all platform logs from `us-east` are routed to `0b4e6aa9-257c-4a3a-ae42-9a36a5b3adc7`. All platform logs from `eu-de` and `eu-es` are routed to `6f9137b3-2bb9-4724-851b-153e23c82d80`.

```text
'[{"action": "send", "targets":[{"id":"0b4e6aa9-257c-4a3a-ae42-9a36a5b3adc7"}], "inclusion_filters":[{"operand": "location","operator": "is","values": ["us-east"]}]},{"targets":[{"id":"22222222-2222-2222-2222-222222222222"},{"id":"33333333-3333-3333-3333-3333333333333"}], "inclusion_filters":[{"operand": "location","operator": "in","values": ["eu-de","eu-es"]}]}]'
```
{: codeblock}

'[{"action": "send", "targets":[{"id":"0b4e6aa9-257c-4a3a-ae42-9a36a5b3adc7"}], "inclusion_filters":[{"operand": "location","operator": "is","values": ["us-east"]}]},{"targets":[{"id":"6f9137b3-2bb9-4724-851b-153e23c82d80"}], "inclusion_filters":[{"operand": "location","operator": "in","values": ["eu-de","eu-es"]}]}]'

The output of the command looks as follows:

```text
Id           92f4cdb3-212c-4811-bf16-03b5449e0fb8
Name         route1
Rules
             action              send
             inclusion_filters
                                 operand    location
                                 operator   is
                                 values     [us-east]

             targets
                                 id            0b4e6aa9-257c-4a3a-ae42-9a36a5b3adc7
                                 name          cl-target2
                                 target_type   cloud-logs


             action              send
             inclusion_filters
                                 operand    location
                                 operator   in
                                 values     [eu-de, eu-es]

             targets
                                 id            6f9137b3-2bb9-4724-851b-153e23c82d80
                                 name          cl-platform-logs-sydney-113
                                 target_type   cloud-logs


Created At   2026-04-28T22:26:38.022Z
Updated At   2026-04-28T22:26:38.022Z
Managed By   account
```
{: screen}

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
        "id":"0b4e6aa9-257c-4a3a-ae42-9a36a5b3adc7""
      },
      {
        "id":"6f9137b3-2bb9-4724-851b-153e23c82d80"
      }
    ],
    "inclusion_filters": [
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





## Define routing rules by using the API
{: #define_rules_api}
{: api}

Targets and inclusion filters are defined in API calls by using `rules`. The `rules` are defined in a JSON structure.

For example, the following example creates a route that is named `my-route` and sends platform logs from the `us-east` region to targets `0b4e6aa9-257c-4a3a-ae42-9a36a5b3adc7` and `6f9137b3-2bb9-4724-851b-153e23c82d80`.

```sh
curl -X POST https://api.private.<REGION>.logs-router.cloud.ibm.com/v3/routes -H "Authorization: Bearer <IAM_TOKEN>" -H 'content-type: application/json' -d '{
    "name": "my-route",
    "rules": [
      {
        "action":"send",
        "targets": [{"id":"0b4e6aa9-257c-4a3a-ae42-9a36a5b3adc7"}, {"id":"6f9137b3-2bb9-4724-851b-153e23c82d80"}],
        "inclusion_filters": [
          {
            "operand": "location",
            "operator": "is",
            "values": ["us-east"]
          }
        ]
      }
    ]
  }'
```
{: codeblock}


For more information, see the [API reference.](https://{DomainName}/apidocs/logs-router/logs-router-v3)
