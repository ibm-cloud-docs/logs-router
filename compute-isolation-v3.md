---

copyright:
  years:  2023, 2026
lastupdated: "2026-04-30"

keywords:

subcollection: logs-router

---

{{site.data.keyword.attribute-definition-list}}




# Learning about {{site.data.keyword.logs_routing_full_notm}} architecture and workload isolation (V3)
{: #compute-isolation-v3}

Review the following sample architecture for {{site.data.keyword.atracker_full}}, and learn more about different isolation levels so that you can choose the solution that best meets the requirements of the workloads that you want to run in the cloud.
{: shortdesc}



## {{site.data.keyword.logs_routing_full_notm}} architecture
{: #architecture}

{{site.data.keyword.logs_routing_full_notm}} is a multi-tenant, regional service that is available in {{site.data.keyword.cloud_notm}}. With {{site.data.keyword.logs_routing_full_notm}}, you can manage collection and storage of platform logs to monitor {{site.data.keyword.cloud_notm}} services in your account.


The following figure shows the high level architecture for {{site.data.keyword.logs_routing_full_notm}}:

![A diagram that shows a sample {{site.data.keyword.logs_routing_full_notm}} architecture.](/images/logs_router_arch.jpg "{{site.data.keyword.logs_routing_full_notm}} architecture sample."){: caption="{{site.data.keyword.logs_routing_full_notm}} sample architecture" caption-side="bottom"}


{{site.data.keyword.logs_routing_full_notm}} is deployed and managed per region. See [List of supported regions](/docs/atracker?topic=atracker-regions). In each region, the service runs in three physically separate data centers to ensure availability.

All data and the configuration for each service deployment is retained within the region in which it is hosted.

You can use the following to manage the service in your account:

* The {{site.data.keyword.logs_routing_full_notm}} UI

* The {{site.data.keyword.logs_routing_full_notm}} CLI

* The {{site.data.keyword.logs_routing_full_notm}} API

* The {{site.data.keyword.logs_routing_full_notm}} Terraform

You must define targets and routes to define how to manage platform logs per region in your account.
- A target is a resource where you can collect platform logs.
- A route defines the rules that determine where platform logs that are genererated in the account are routed.

You can define account settings to define global configuration parameters that apply when you configure the account.

In your account, platform logs are automatically collected from {{site.data.keyword.cloud_notm}} services that run in the account, with the exception of some services that require additional configuration to enable platform logs. For more information about services that generate events, see [Cloud services](/docs/logs-router?topic=logs-router-cloud_services).

After you configure targets and routes in the account, platform logs that are collected are uploaded to the destination target of your choice. You are responsible for managing the platform logs in the target resources.




## Connections
{: #compute-isolation-connections}

You can use private and public endpoints to configure {{site.data.keyword.logs_routing_full_notm}} resources in your account.

### Private connections
{: #compute-isolation-private-connections}

You cannot disable private endpoints.
{: note}


### Public connections
{: #compute-isolation-public-connections}

You can choose to disable public endpoints for {{site.data.keyword.logs_routing_full_notm}}.

Disabling public endpoints will disable the {{site.data.keyword.logs_routing_full_notm}} UI.
{: important}

For more information, see [Enforcing private endpoints to configure {{site.data.keyword.logs_routing_full_notm}} resources](/docs/logs-router?topic=logs-router-getting-started-mng-endpoints).


## Dependencies to other {{site.data.keyword.cloud_notm}} services
{: #compute-isolation-dependencies-cloud}

Review the {{site.data.keyword.cloud_notm}} services that {{site.data.keyword.logs_routing_full_notm}} connects to over public or private connections.

| Service name | Description |
|------------|-------------------------------------|
| {{site.data.keyword.cis_full_notm}} | {{site.data.keyword.cis_full_notm}} is used as a provider for DNS and load-balancing capabilities. |
| {{site.data.keyword.containerlong_notm}} | {{site.data.keyword.logs_routing_full_notm}} uses {{site.data.keyword.containerlong_notm}} to run its service. |
| {{site.data.keyword.mon_full_notm}} | {{site.data.keyword.logs_routing_full_notm}} integrates with {{site.data.keyword.mon_short}}, by using a private connection, to send platform metrics. For more information, see [Monitoring metrics for {{site.data.keyword.logs_routing_full_notm}}](/docs/atracker?topic=atracker-monitoring_metrics). |
| {{site.data.keyword.cos_full_notm}} | {{site.data.keyword.logs_routing_full_notm}} stores customer data in {{site.data.keyword.cos_short}} by using a private connection. All data is encrypted in transit and at rest. For more information, see [Managing your data in {{site.data.keyword.logs_routing_full_notm}}](/docs/atracker?topic=atracker-mng-data).|
| {{site.data.keyword.cloud_notm}} Platform | To authenticate requests to the service and authorize user actions, {{site.data.keyword.logs_routing_full_notm}} implements platform and service access roles in {{site.data.keyword.iamshort}} (IAM). For more information about required IAM permissions to work with the service, see [Managing access for {{site.data.keyword.logs_routing_full_notm}}](/docs/atracker?topic=atracker-iam). Connections from {{site.data.keyword.logs_routing_full_notm}} to IAM do not use private connections. |
| {{site.data.keyword.databases-for-postgresql_full_notm}} | {{site.data.keyword.logs_routing_full_notm}} uses {{site.data.keyword.databases-for-postgresql_full_notm}}  for storing metadata. |
| {{site.data.keyword.logs_full_notm}} | {{site.data.keyword.logs_routing_full_notm}} routes customer data to {{site.data.keyword.logs_full_notm}} by using a private connection. All data is encrypted in transit and at rest. For more information, see [Managing your data in {{site.data.keyword.logs_routing_full_notm}}](/docs/atracker?topic=atracker-mng-data).|
{: caption="{{site.data.keyword.logs_routing_full_notm}} dependencies to other {{site.data.keyword.cloud_notm}} services." caption-side="top"}
{: summary="The first column is the service. The second column is a description of the service."}



## Workload isolation
{: #compute-isolation-workload}


Each regional deployment serves multiple tenants that are identified by the {{site.data.keyword.cloud_notm}} account ID.

- There is 1 deployment per region that is responsible for running user workloads in the region.
- In a region, the deployment is highly available.
- The data that is collected is associated with the {{site.data.keyword.cloud_notm}} account ID and not visible to the other users by virtue of this association.
- Data for all tenants is co-located in the same data stores and segmented by the tenant-specific {{site.data.keyword.cloud_notm}} account ID to enforce access control policies.
- You can use IBM Cloud Identity and Access Management (IAM) to control which users see, create, use, and manage resources.
