---

copyright:
  years:  2023, 2024
lastupdated: "2024-11-18"

keywords:

subcollection: logs-router

---

{{site.data.keyword.attribute-definition-list}}

# Learning about {{site.data.keyword.logs_routing_full_notm}}
{: #about}

You can use the {{site.data.keyword.logs_routing_full_notm}} service to route platform logs that are generated by selected {{site.data.keyword.cloud_notm}} service instances to your chosen destination (target).
{: shortdesc}

![Flow of platform logs](/images/cloud-logs-platform-logs.png "Flow of platform logs"){: caption="Flow of platform logs" caption-side="bottom"}

For more information, see [About platform logs](/docs/logs-router?topic=logs-router-about-platform-logs).


To configure platform logs, you must configure tenants and targets (destinations) in your {{site.data.keyword.cloud_notm}} account.

{{site.data.content.tenant_definition-paragraph}}


The following figure shows a high level view of the components and how they relate to each other:

![High level view of the components](/images/components-ov.png "High level view of the components"){: caption="High level view of the components" caption-side="bottom"}



## Tenants
{: #about_tenants}


When you manage tenants, consider the following information:

- You must create a tenant in your account in each region where you want to use {{site.data.keyword.logs_routing_full_notm}} to collect and route platform logs.

    Each region is independent.

    Regions do not share data.

- When you create a tenant, you must define 1 target destination. For more information, see [Creating a tenant](/docs/logs-router?topic=logs-router-tenant-create).
- You can define up to 2 target destinations per target. The targets must be of different type.
- When you update a tenant, you can modify the name of the tenant only.
- When you delete a tenant, you also delete the target definitions. If you want to delete a target destination instance, you must delete the instance separately.
- You can list all the tenants in an account by using the `v1/tenants` route.
- To get the details of a tenant, the route accepts a query parameter name to return details of the named tenant.

## Targets
{: #about_targets}

{{site.data.keyword.logs_routing_full_notm}} supports the following types of targets:
- {{site.data.keyword.logs_full_notm}} instances

    Only public ingress endpoints are supported when configuring a target to an {{site.data.keyword.logs_full_notm}} instance.
    {: restriction}

    For more information, see [Configuring the {{site.data.keyword.logs_routing_full_notm}} service to route platform logs to an {{site.data.keyword.logs_full_notm}} instance](/docs/logs-router?topic=logs-router-target-cloud-logs).

- {{site.data.keyword.la_full_notm}} instances

    For more information, see [Configuring the {{site.data.keyword.logs_routing_full_notm}} service to route platform logs to an {{site.data.keyword.la_full_notm}} instance](/logs-router?topic=logs-router-onboard-log-analysis-tenant).

The {{site.data.keyword.logs_routing_full_notm}} target instance can be located in the same account, a different account, and the same or different region as the {{site.data.keyword.logs_routing_full_notm}} tenant.
{: important}

When you manage targets, consider the following information:

- When you create a target, you can only add 1 target.

    When tenant is created, at least one target has to be specified. You can only define 2 targets per tenant. Therefore, you can only add 1 target to an existing tenant. {: note}

    The target name must be unique within the region and not exceed 35 characters in length. If you attempt to create a target with a name that already exists within the region or is too long, an error is returned and your target is not created.
    {: note}

- You can update the target configuration details such as target host, target port, target name, target instance CRN, and more.

- You can delete only 1 target from a tenant.

- You can list all the targets in a tenant by using the `v1/tenants/tenantID/targets` route.

- To get the details of a target, the route accepts a query parameter name to return details of the named target.



Authorization between the {{site.data.keyword.logs_routing_full_notm}} service and the target destination instances is done as follows:
- For {{site.data.keyword.logs_full_notm}} targets, authorization is done through IAM.
- For {{site.data.keyword.la_full_notm}} targets, you must provide ingestion details that include the credentials.


## Connecting to {{site.data.keyword.logs_routing_full_notm}}
{: #about_connecting}

{{site.data.keyword.logs_routing_full}} provides API endpoints for  management functions, such as creating or managing a tenant configuration. These API endpoints are endpoints are accessed by using region-specific URLs. You can find the endpoints for each supported region [here](/docs/logs-router?topic=logs-router-endpoints).

You can manage {{site.data.keyword.logs_routing_full_notm}} by using the management API. The management API supports either a public endpoint or a private endpoint. A public endpoint can be reached over the internet, whereas a private endpoint can be accessed only from within the {{site.data.keyword.cloud_notm}} private network.



Through the {{site.data.keyword.logs_routing_full_notm}} UI, you can create a tenant in a region by using the public network.



![Flow of routed logs](/images/Logs-Router-04--1.svg "Resources"){: caption="Resources" caption-side="bottom"}


The {{site.data.keyword.logs_routing_full}} supports the following types of endpoints to privately connect to {{site.data.keyword.logs_routing_full_notm}}:
- {{site.data.keyword.cloud_notm}} Cloud Service Endpoint (CSE)
- {{site.data.keyword.vpe_full}} for VPC.

If you are connecting from an {{site.data.keyword.vpc_short}}, you can connect by using either a CSE or {{site.data.keyword.vpe_short}}. For more information, see [Using virtual private endpoints for VPC to privately connect to {{site.data.keyword.logs_routing_full_notm}}](/docs/logs-router?topic=logs-router-vpe-connection&interface=api).

If you are connecting from a system that is not contained in an {{site.data.keyword.vpc_short}}, your only private endpoint option is to connect by using a CSE. Connecting to {{site.data.keyword.logs_routing_full_notm}} by using a CSE might not require any additional work on your part. Access to CSEs is only available from within the {{site.data.keyword.cloud_notm}} private network. For more information, see [Using service endpoints to privately connect to {{site.data.keyword.logs_routing_full_notm}}](/docs/logs-router?topic=logs-router-service-endpoints).

You can only access the {{site.data.keyword.logs_routing_full_notm}} UI through the public network.


You can manage {{site.data.keyword.logs_routing_full_notm}} by using the management API. The management API supports either a public endpoint or a private endpoint. A public endpoint can be reached over the internet, whereas a private endpoint can be accessed only from within the {{site.data.keyword.cloud_notm}} private network.

- To create, update, or delete a configuration, you must use the {{site.data.keyword.logs_routing_full_notm}} management API.
- To see the list of management endpoints, see [Endpoints](/docs/logs-router?topic=logs-router-endpoints).

Using the public endpoint does not require additional work.
