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

You can configure exactly where {{site.data.keyword.logs_routing_full_notm}} routes platform metrics in your account. Route rule inclusion filters provides elevated control over how your metrics are routed. You can also configure default targets by using {{site.data.keyword.logs_routing_full_notm}} settings. How the configuration is processed determines the final destination where metrics are sent; each data point is processed individually.
{: shortdesc}

1. If a route rule's inclusion filters match the data point, the data point is sent to the configured targets. Route rules are processed sequentially, the first match is used. If there are multiple routes, all route rules will be processed to find a match.

    If a matched route rule uses the `drop` action, the data point will be dropped and no destinations will receive it.

2. If the data point does not match any route rules, the [default targets setting](/docs/metrics-router?topic=metrics-router-target-default) is used to route the data point to the default targets.

3. If the data point does not match any route rules, and no default target is defined, the metric is routed to the {{site.data.keyword.mon_full_notm}} instance defined in the region as the receiver of platform metrics. [The {{site.data.keyword.mon_full_notm}} instance enabled to receive platform metrics must exist in the region to receive the metrics.](/docs/monitoring?topic=monitoring-platform_metrics_enabling)
