---

copyright:
  years:  2023, 2026
lastupdated: "2026-04-29"

keywords:

subcollection: logs-router

---

{{site.data.keyword.attribute-definition-list}}


# Understanding routing precedence
{: #routes_precedence}

By configuring {{site.data.keyword.logs_routing_full_notm}} routing rules, you can specify how and where platform logs are routed in your account.
- You can use route rule inclusion filters to provide elevated control over how your platform logs are routed.
- You can configure default targets by using {{site.data.keyword.logs_routing_full_notm}} settings to collect logs that are generated in the account and do not have a routing rule associated to define where they are routed
.
How the configuration is processed determines the final destination where platform logs are sent; each platform log is processed individually.

## How are routes and rules applied
{: #routes_precedence_how}

1. Routes are processed independently.

    If platform logs match rules in multiple routes, those logs will be sent to all targets attached to the matched rules.

2. Route rules are processed in order.

    Route rules are processed sequentially. The first match is used.

    Once a platform log matches a rule within a route, it will not be processed by subsequent rules in that same route.

    If a match is found and the rule specifies to drop logs, the logs are discarded.

3. Unmatched platform logs are discarded.

    Platform logs that do not meet any criteria are sent to the default targets. For more information, see [default targets setting](/docs/logs-router?topic=logs-router-target-default).

    If no default target is set, logs are not routed.
