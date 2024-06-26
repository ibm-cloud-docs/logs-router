---

copyright:
  years:  2022, 2023
lastupdated: "2024-03-07"

keywords:

subcollection: logs-router

---

{{site.data.keyword.attribute-definition-list}}

# Learning about {{site.data.keyword.logs_routing_full_notm}}
{: #about}

You can use the {{site.data.keyword.logs_routing_full_notm}} service to route logs from your {{site.data.keyword.cloud_notm}} account to your chosen target. You can route logs from your own {{site.data.keyword.cloud_notm}} workloads, such as, applications on your [{{site.data.keyword.containerlong_notm}}](/docs/containers) or [{{site.data.keyword.openshiftlong_notm}}](/docs/openshift) clusters, and from selected {{site.data.keyword.cloud_notm}} service instances.
{: shortdesc}


![Flow of routed logs](../images/get-started.png "Flow of routed logs"){: caption="Figure 1. Flow of routed logs" caption-side="bottom"}

## Tenants and targets
{: #about_tenant_target}

{{site.data.keyword.logs_routing_full_notm}} uses tenants and targets.

{{site.data.content.tenant_definition-paragraph}}

You must create (onboard) a tenant in your account in each region where you want to use {{site.data.keyword.logs_routing_full_notm}}. Each region is independent and regions do not share data.

{{site.data.keyword.logs_routing_full_notm}} supports the following {{site.data.keyword.cloud_notm}} services as targets:
- {{site.data.keyword.la_full_notm}}
- {{site.data.keyword.logs_full_notm}}
Instances can be in the same account, a different account, and the same or different region as the {{site.data.keyword.logs_routing_full_notm}} tenant.




## Connecting to {{site.data.keyword.logs_routing_full_notm}}
{: #about_connecting}

{{site.data.keyword.logs_routing_full}} provides API endpoints for both management functions, such as creating (onboarding) as a tenant, and ingestion of logs. These API endpoints are separate endpoints that are accessed by using specific URLs in each supported region. You can find the endpoints for each supported region [here](/docs/logs-router?topic=logs-router-endpoints).

You can manage {{site.data.keyword.logs_routing_full_notm}} by using the management API. The management API supports either a public endpoint or a private endpoint. A public endpoint can be reached over the internet, whereas a private endpoint can be accessed only from within the {{site.data.keyword.cloud_notm}} private network.

You can send logs to a target destination by using the ingestion API. The ingestion API supports only private endpoints and is therefore not accessible from the public internet.

Through the {{site.data.keyword.logs_routing_full_notm}} UI, you can create a tenant in a region by using the public network.

You must ensure that you can connect to both the management and ingestion endpoints.
{: note}


![Flow of routed logs](../images/Logs-Router-04--1.svg "Resources"){: caption="Figure 1. Resources" caption-side="bottom"}


The {{site.data.keyword.logs_routing_full}} supports the following types of endpoints to privately connect to {{site.data.keyword.logs_routing_full_notm}}:
- {{site.data.keyword.cloud_notm}} Cloud Service Endpoint (CSE)
- {{site.data.keyword.vpe_full}} for VPC.

If you are connecting from an {{site.data.keyword.vpc_short}}, you can connect by using either a CSE or {{site.data.keyword.vpe_short}}. For more information, see [Using virtual private endpoints for VPC to privately connect to {{site.data.keyword.logs_routing_full_notm}}](/docs/logs-router?topic=logs-router-vpe-connection&interface=api).

If you are connecting from a system that is not contained in an {{site.data.keyword.vpc_short}}, your only private endpoint option is to connect by using a CSE. Connecting to {{site.data.keyword.logs_routing_full_notm}} by using a CSE might not require any additional work on your part. Access to CSEs is only available from within the {{site.data.keyword.cloud_notm}} private network. For more information, see [Using service endpoints to privately connect to {{site.data.keyword.logs_routing_full_notm}}](/docs/logs-router?topic=logs-router-service-endpoints).

You can only access the {{site.data.keyword.logs_routing_full_notm}} UI through the public network.


## Log sources
{: #about_sources}

Logs can be received by {{site.data.keyword.logs_routing_full}} from two sources:

* Log data sent to {{site.data.keyword.logs_routing_full}} by {{site.data.keyword.cloud_notm}} [services that support {{site.data.keyword.logs_routing_full}}.](/docs/logs-router?topic=logs-router-cloud_services_locations).

* Applications with a running {{site.data.keyword.logs_routing_full}} agent on [{{site.data.keyword.containerlong_notm}}](/docs/containers) or [{{site.data.keyword.openshiftlong_notm}}](/docs/openshift) clusters.




## Determining the origin of {{site.data.keyword.cloud_notm}} log data
{: #about_data_origin}

You can determine the {{site.data.keyword.cloud_notm}} service routing log data by the `logSourceCRN` value that is included in the log line. The `logSourceCRN` value is the [Cloud Resource Name (CRN)](/docs/account?topic=account-crn) of the originating {{site.data.keyword.cloud_notm}} service.

This value is not provided for logs that are sent by the {{site.data.keyword.logs_routing_full_notm}} agent.
{: note}
