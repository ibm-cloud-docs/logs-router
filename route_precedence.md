---

copyright:
  years:  2023, 2026
lastupdated: "2026-04-28"

keywords:

subcollection: logs-router

---

{{site.data.keyword.attribute-definition-list}}


# Understanding routing precedence
{: #routes_precedence}

You can configure precisely where {{site.data.keyword.logs_routing_full_notm}} routes platform logs in your account.
- You can use route rule inclusion filters to provide elevated control over how your platform logs are routed.
- You can configure default targets by using {{site.data.keyword.logs_routing_full_notm}} settings to collect logs that are generated in the account and do not have a routing rule associated to define where they are routed
.
How the configuration is processed determines the final destination where platform logs are sent; each platform log is processed individually.

1. If a route rule's inclusion filters match the platform log, the platform log is sent to the configured targets. Route rules are processed sequentially, the first match is used. If there are multiple routes, all route rules will be processed to find a match.

    If a matched route rule uses the `drop` action, the platform log will be dropped and no destinations will receive it.

2. If the platform log does not match any route rules, the [default targets setting](/docs/logs-router?topic=logs-router-target-default) is used to route the platform log to the default targets.

3. If the platform log does not match any route rules, and no default target is defined, the platform log is dropped.
