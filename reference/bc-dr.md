---

copyright:
  years: 2023
lastupdated: "2023-11-29"

keywords:

subcollection: logs-router

---

{{site.data.keyword.attribute-definition-list}}

# Understanding business continuity and disaster recovery for {{site.data.keyword.logs_routing_full_notm}}
{: #bc-dr}

[Disaster recovery](#x2113280){: term} involves a set of policies, tools, and procedures for returning a system, an application, or an entire data center to full operation after a catastrophic interruption. It includes procedures for copying and storing an installed system's essential data in a secure location, and for recovering that data to restore normalcy of operation.
{: shortdesc}

## Responsibilities
{: #bc-dr-responsibilities}

For more information about your responsibilities when you are using {{site.data.keyword.logs_routing_full_notm}}, see [Shared responsibilities for {{site.data.keyword.logs_routing_full_notm}}](/docs/logs-router?topic=logs-router-shared-responsibilities).

## Disaster recovery strategy
{: #bc-dr-strategy}

{{site.data.keyword.cloud_notm}} has [business continuity](#x3026801){: term} plans in place to provide for the recovery of services within hours if a disaster occurs. You are responsible for your data backup and associated recovery of your content.

{{site.data.keyword.logs_routing_full_notm}} provides ways to protect your data and restore service functions. Business continuity plans are in place to achieve targeted [recovery point objective](#x3429911){: term} (RPO) and [recovery time objective](#x3167918){: term} (RTO) for the service. The following table outlines the targets for {{site.data.keyword.logs_routing_full_notm}}.

| Disaster recovery objective | Target Value   |
|---|---|
|  RPO |  Within 24 hours |
|  RTO |  Within 24 hours |
{: caption="Table 1. RPO and RTO for {{site.data.keyword.logs_router_notm}}" caption-side="bottom"}

{{site.data.keyword.logs_routing_full_notm}} data includes information about the targets where logging events are delivered for tenants that are onboarded to the region. A target is a resource where logging events are collected.

{{site.data.keyword.logs_routing_full_notm}} regularly backups the data in each region:

- Regular backups are done daily and retained for 30 days.

- Continuous incremental backups are kept for the last 7 days.

{{site.data.keyword.logs_routing_full_notm}} data is replicated across multiple regions. Regular backups are stored across multiple regions and can be restored to other regions.

## Locations
{: #bcdr-ha-locations}

For more information about service availability within regions and data centers, see [Service and infrastructure availability by location](/docs/overview?topic=overview-services_region).

## Disaster recovery (DR) in a region
{: #bc-dr-recovery}

In the case of a regional disaster, you must complete the following steps to establish cross-region high availability:

1. Identify an alternate regional {{site.data.keyword.logs_routing_full_notm}} deployment.

2. If an {{site.data.keyword.logs_routing_full_notm}} tenant is not created (onboarded) in the alternate region, you must configure {{site.data.keyword.logs_routing_full_notm}} in the alternate region. [Learn more](/docs/logs-router?topic=logs-router-getting-started).

3. Reconfigure any deployed {{site.data.keyword.logs_routing_full_notm}} agents to use the alternate region's [ingestion endpoint.](/docs/logs-router?topic=logs-router-endpoints)

When {{site.data.keyword.logs_routing_full_notm}} recovers in the region that is down, your configuration is restored. You can reconfigure any agents set to use the alternate region for ingestion to use the original region.
{: important}
