---

copyright:
  years:  2023, 2024
lastupdated: "2024-12-10"

keywords:

subcollection: logs-router

---

{{site.data.keyword.attribute-definition-list}}

# Understanding high availability and disaster recovery for {{site.data.keyword.logs_routing_full_notm}}
{: #logs-router-ha-dr}

[High availability](#x2284708){: term} (HA) is the ability for a service to remain operational and accessible in the presence of unexpected failures. [Disaster recovery](#x2113280){: term} is the process of recovering the service instance to a working state.
{: shortdesc}

{{site.data.keyword.logs_routing_full}} is a highly available, multi-tenant, regional service. Learn more about the {{site.data.keyword.logs_routing_full_notm}} service's availability and disaster recovery strategies.

## Service high availability (HA)
{: #ha_svc_availability}

An availability zone is a logically and physically isolated location within an {{site.data.keyword.cloud_notm}} region where your data is processed and hosted.
* An availability zone has independent power, cooling, and network infrastructures that are isolated from other zones to strengthen fault tolerance by avoiding single points of failure between zones.
* An availability zone offers high bandwidth and low inter-zone latency within a region.

A region (location) is a geographically and physically separate group of one or more availability zones with independent electrical and network infrastructures that are isolated from other regions.
* Regions are designed to remove shared single points of failure with other regions and low inter-zone latency within the region.
* Each region has 3 different data centers (DC) for redundancy.

## Responsibilities
{: #ha-responsibilities}

For more information about your responsibilities when you are using {{site.data.keyword.logs_routing_full_notm}}, see [Shared responsibilities for {{site.data.keyword.logs_routing_full_notm}}](/docs/logs-router?topic=logs-router-shared-responsibilities).






## Locations
{: #ha-locations}

{{site.data.keyword.logs_routing_full_notm}} is a highly available, regional, service.
- {{site.data.keyword.logs_routing_full_notm}} is available in a single region but will become available in other regions over time. For more information about the regions where {{site.data.keyword.logs_routing_full_notm}} is available, see [Locations](/docs/logs-router?topic=logs-router-locations).
- Each region has three different data centers for redundancy that are configured in `active/active` mode.
- If all the data centers in a location fail, {{site.data.keyword.logs_routing_full_notm}} becomes unavailable in that location.
- In each supported region, traffic is load balanced across infrastructure in multiple availability zones, with no single point of failure.


The following table lists the high-availability (HA) status for the regions (locations) where the {{site.data.keyword.logs_routing_full_notm}} service is available:

| Geography             | Region                   | EU-Supported | HA Status |
|-----------------------|--------------------------|--------------|-----------|
| Asia Pacific  | Osaka (`jp-osa`) | NO | MZR       |
| Asia Pacific  | Sydney (`au-syd`) | NO | MZR       |
| Asia Pacific  | Tokyo (`jp-tok`) | NO | MZR       |
| Europe  | Frankfurt (`eu-de`) | NO | MZR       |
| Europe  | London (`eu-gb`) | NO | MZR       |
| Europe  | Madrid (`eu-es`) | NO | MZR       |
| North America  | Dallas (`us-south`) | N/A | MZR       |
| North America  | Toronto (`ca-tor`) | N/A | MZR       |
| North America  | Washington, D.C (`us-east`) | N/A | MZR       |
| South America  | Sao Paulo (`br-sao`) | N/A | MZR       |
{: caption="List of locations where the service is available" caption-side="top"}

Where
* A *geography* is a geographic area or larger political body that contains one or more regions.
* A *region* is a defined geographic territory.

    A region might be a specific postal code area, a town, a city, a state, a group of states, or even a group of countries.

    A region contains [multiple availability zones](https://www.ibm.com/cloud/data-centers/) to meet local access, low latency, and security requirements for the region.

* `N/A` means feature that is not applicable to that geography.
* `MZR` means multi-zone region. [Learn more](/docs/overview?topic=overview-locations#table-mzr).

For more information about service availability within regions and data centers, see [Service and infrastructure availability by location](/docs/overview?topic=overview-services_region).

## Data availability
{: #ha-data_availability}

The data that is managed by {{site.data.keyword.logs_routing_full_notm}} in a region is kept in the data centers near that region.

A multizone region (MZR) consists of 3 or more availability zones that are independent from each other to ensure that that single failure events affect only a single zone.

By default, {{site.data.keyword.logs_routing_full_notm}} is deployed across 3 zones. Each zone is set up with `active/active/active`:
* Each zone is located in a different data center in the region.
* The data in each zone is automatically replicated to the other zones with low latency. You do not need to do anything to enable the replication.
* The service is designed to withstand a single zone failure with no interruption.

The MZR architecture offers automatic failover between zones within the region, and high availability for a log routing deployment within a region.

{{site.data.keyword.logs_routing_full_notm}} data includes information on the targets where logging events are delivered for tenants created (onboarded) in that region. A target is a resource where you can collect logging events.


## Disaster recovery strategy
{: #bc-dr-strategy}

{{site.data.keyword.cloud_notm}} has [business continuity](#x3026801){: term} plans in place to provide for the recovery of services within hours if a disaster occurs. You are responsible for your data backup and associated recovery of your content.

{{site.data.keyword.logs_routing_full_notm}} provides ways to protect your data and restore service functions. Business continuity plans are in place to achieve targeted [recovery point objective](#x3429911){: term} (RPO) and [recovery time objective](#x3167918){: term} (RTO) for the service. The following table outlines the targets for {{site.data.keyword.logs_routing_full_notm}}.

| Disaster recovery objective | Target Value   |
|---|---|
|  RPO |  Within 24 hours |
|  RTO |  Within 24 hours |
{: caption="RPO and RTO for {{site.data.keyword.logs_router_notm}}" caption-side="bottom"}

{{site.data.keyword.logs_routing_full_notm}} data includes information about the targets where logging events are delivered for tenants that are onboarded to the region. A target is a resource where logging events are collected.

{{site.data.keyword.logs_routing_full_notm}} regularly backups the data in each region:

- Regular backups are done daily and retained for 30 days.

- Continuous incremental backups are kept for the last 7 days.

{{site.data.keyword.logs_routing_full_notm}} data is replicated across multiple regions. Regular backups are stored across multiple regions and can be restored to other regions.

## Disaster recovery (DR) in a region
{: #bc-dr-recovery}

In the case of a regional disaster, you must complete the following steps to establish cross-region high availability:

1. Identify an alternate regional {{site.data.keyword.logs_routing_full_notm}} deployment.

2. If an {{site.data.keyword.logs_routing_full_notm}} tenant is not created (onboarded) in the alternate region, you must configure {{site.data.keyword.logs_routing_full_notm}} in the alternate region. [Learn more](/docs/logs-router?topic=logs-router-getting-started).

3. Reconfigure any deployed {{site.data.keyword.logs_routing_full_notm}} agents to use the alternate region's [ingestion endpoint.](/docs/logs-router?topic=logs-router-endpoints)

When {{site.data.keyword.logs_routing_full_notm}} recovers in the region that is down, your configuration is restored. You can reconfigure any agents set to use the alternate region for ingestion to use the original region.
{: important}
