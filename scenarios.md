---

copyright:
  years: "2026"
lastupdated: "2026-04-29"

keywords:

subcollection: logs-router

---

{{site.data.keyword.attribute-definition-list}}

# {{site.data.keyword.logs_routing_full_notm}} configuration scenarios (V3)
{: #scenarios}

You can configure a centralize logging model, a geographic logging model or regional logging model when you setup {{site.data.keyword.logs_routing_full_notm}}.
{: shortdesc}

The following scenarios represent the most common logging configurations and will help you determine the appropriate setup for your v3 environment.

## Scenario 1: Centralized Logging
{: #scenarios-1}

In a centralized logging configuration, all platform logs for your entire account are consolidated into a single {{site.data.keyword.logs_full_notm}} instance.

This approach simplifies log management by providing a unified view of all platform activities across your account.

To implement this scenario in v3, you will need to create one target pointing to your centralized {{site.data.keyword.logs_full_notm}} instance and [configure one route with a wildcard rule](/docs/logs-router?topic=logs-router-route-rule-all-logs) to capture all platform logs regardless of their source region.

### Scenario 2: Geographic Logging
{: #scenarios-2}

The geographic logging scenario is designed for organizations that maintain multiple logging instances distributed across different geographic locations.

In this configuration, platform logs from various regions are routed to their nearest or designated regional {{site.data.keyword.logs_full_notm}} instance based on geographic proximity or data residency requirements. This approach balances centralized visibility with geographic distribution, allowing you to route logs from multiple regions to a smaller number of strategically located {{site.data.keyword.logs_full_notm}} instances.

To implement this scenario, you will need to create multiple targets (one for each geographic {{site.data.keyword.logs_full_notm}} instance) and configure routes with appropriate region-based filters to direct logs to the correct geographic destination.

### Scenario 3: Regional Logging
{: #scenarios-3}

Regional logging represents the most distributed approach, where each {{site.data.keyword.cloud_notm}} region has its own dedicated {{site.data.keyword.logs_full_notm}} instance.

This configuration ensures complete regional isolation of log data and is often used to meet strict data residency or compliance requirements. In this scenario, platform logs generated in a specific region are routed exclusively to the {{site.data.keyword.logs_full_notm}} instance deployed in that same region.

To implement this scenario in v3, you will need to create a separate target and route for each region where you operate, ensuring that each regional route includes filters that restrict log routing to only that region's {{site.data.keyword.logs_full_notm}} instance.
