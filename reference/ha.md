---

copyright:
  years: 2023, 2024
lastupdated: "2024-04-29"

keywords:

subcollection: logs-router

---

{{site.data.keyword.attribute-definition-list}}

# Understanding high availability for {{site.data.keyword.logs_routing_full_notm}}
{: #ha}

{{site.data.keyword.logs_routing_full}} is a highly available, multi-tenant, regional service. Learn more about the {{site.data.keyword.logs_routing_full_notm}} service's availability and disaster recovery strategies.
{: shortdesc}

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

See [Your responsibilities when using {{site.data.keyword.logs_routing_full_notm}}](/docs/logs-router?topic=logs-router-shared-responsibilities).

<!-- ## What level of availability do I need?
{: #ha-level}

You can achieve high availability on different levels in your IT infrastructure and within different components of your cluster. The level of availability that is right for you depends on several factors, such as your business requirements, the service level agreements (SLAs) that you have with your customers, and the resources that you want to expend.-->

<!-- ## What level of availability does {{site.data.keyword.cloud_notm}} offer?
{: #ha-service}

The level of availability that you set up for your cluster impacts your coverage under the {{site.data.keyword.cloud_notm}} high availability service level agreement terms.

Service level objectives (SLOs) describe the design points that the {{site.data.keyword.cloud_notm}} services are engineered to meet. _service-name_ is designed to achieve the following availability target.

| Availability target | Target Value   |
|---|---|
|  Availability % |   |
{: caption="Table 1. SLO for _service-name_" caption-side="bottom"}

The SLO is not a warranty and {{site.data.keyword.IBM_notm}} will not issue credits for failure to meet an objective. Refer to the SLAs for commitments and credits that are issued for failure to meet any committed SLAs. For a summary of all SLOs, see [{{site.data.keyword.cloud_notm}} service level objectives](/docs/overview?topic=overview-slo). -->


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
{: caption="Table 1. List of locations where the service is available" caption-side="top"}

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
