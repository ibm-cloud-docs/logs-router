---

copyright:
  years:  2023, 2025
lastupdated: "2025-11-17"

keywords:

subcollection: logs-router

---

{{site.data.keyword.attribute-definition-list}}


# Using virtual private endpoints for VPC to privately connect to {{site.data.keyword.logs_routing_full_notm}}
{: #vpe-connection}

{{site.data.keyword.cloud}} Virtual Private Endpoints (VPE) for VPC enables you to connect to {{site.data.keyword.logs_routing_full_notm}} from your VPC network by using the IP addresses of your choosing, allocated from a subnet within your VPC.
{: shortdesc}

VPEs are virtual IP interfaces that are bound to an endpoint gateway created on a per service, or service instance, basis (depending on the service operation model). The endpoint gateway is a virtualized function that scales horizontally, is redundant and highly available, and spans all availability zones of your VPC. Endpoint gateways enable communications from virtual server instances within your VPC and {{site.data.keyword.cloud}} services on the private backbone. VPE for VPC gives you the experience of controlling all the private addressing within your cloud. For more information, see [About virtual private endpoint gateways](/docs/vpc?topic=vpc-about-vpe).

VPE for VPC provides private connectivity to IBM services such as {{site.data.keyword.logs_routing_full_notm}}, but within the VPC network of your choosing.

{{site.data.keyword.logs_routing_full}} provides API endpoints for both management functions, such as creating (onboarding) as a tenant, and ingestion of logs. These API endpoints are separate endpoints that are accessed by using specific URLs in each supported region. You can find the endpoints for each supported region [here](/docs/logs-router?topic=logs-router-endpoints).

To connect to {{site.data.keyword.logs_routing_full_notm}} in a region through private endpoints, you must define a VPE for managing the service and a different VPE for ingesting logs into the service so that you can connect to {{site.data.keyword.logs_routing_full_notm}} within the VPC network of your choosing.
{: note}


## Before you begin
{: #vpe-connection-prereqs}

Before you target a virtual private endpoint for {{site.data.keyword.logs_routing_full_notm}}, you must complete the following tasks:

* Ensure that a [Virtual Private Cloud is created](/docs/vpc?topic=vpc-getting-started).
* Make a plan for your [virtual private endpoints](/docs/vpc?topic=vpc-vpe-planning-considerations).
* Ensure that [correct access controls](/docs/vpc?topic=vpc-configure-acls-sgs-endpoint-gateways) are set for your virtual private endpoint.
* Understand the [limitations](/docs/vpc?topic=vpc-limitations) of VPC.
* Understand how to [view details](/docs/vpc?topic=vpc-vpe-viewing-details-of-an-endpoint-gateway) about a virtual private endpoint.



## Setting up a VPE for VPC
{: #vpe-connection-setup}

To configure a virtual private endpoint in a region to enable connectivity to the {{site.data.keyword.logs_routing_full_notm}} service, follow these steps:

1. Select the **Menu** icon ![Menu icon](../icons/icon_hamburger.svg "Menu"), and click **Infrastructure > VPC Layout > Network > Virtual private endpoint gateways**. Then, click **Create**.

    The **New virtual private endpoint gateway for VPC** page is displayed.

2. Enter the details of the gateway.

    - Enter a name.

    - Choose a resource group. By default, `default` is selected.

    - Add any tags and access management tags.

    - Select the Virtual Private Cloud that the gateway is attached to.

3. In the *Security groups* section, select at least one and at most five security groups to control traffic at the networking level.

4. In the *request connection to a service*, complete the following steps:

    - Select the *Cloud service offering* `IBM Cloud Logs Routing`.

    - Select a region. The VPE gateway region and the *Cloud service region* must be the same. For more information, see [Locations](/docs/logs-router?topic=logs-router-locations).

    - Select an endpoint. You can configure a management and an ingestion VPE for {{site.data.keyword.logs_routing_full_notm}} per region.

        For example, you can select `ingester.private.us-east.logs-router.cloud.ibm.com` to allow routing of logs within this region by using this VPE endpoint.

        When you create a VPE gateway by using the CLI or API, you must specify a [Cloud Resource Name (CRN)](/docs/account?topic=account-crn) for {{site.data.keyword.logs_routing_full_notm}}. Review the following tables for the available CRNs by region. You can configure a management and an ingestion VPE for {{site.data.keyword.logs_routing_full_notm}} per region. For more information on CRN values, see [Cloud Resource Name (CRN) for {{site.data.keyword.logs_routing_full_notm}}](/docs/logs-router?topic=logs-router-vpe-connection-crn).

5. [Bind a reserved IP address](/docs/vpc?topic=vpc-bind-unbind-reserved-ip) to the endpoint gateway.

6. View the created VPE gateway that is associated with the {{site.data.keyword.logs_routing_full_notm}}. For more information, see [Viewing details of an endpoint gateway](/docs/vpc?topic=vpc-vpe-viewing-details-of-an-endpoint-gateway).

Now from your VPC, you can access {{site.data.keyword.logs_routing_full_notm}} privately through the gateway.

For more information, see [Create an endpoint gateway](/docs/vpc?topic=vpc-ordering-endpoint-gateway).


## Using your VPE for {{site.data.keyword.logs_routing_full_notm}}
{: #vpe-connection-using}

After you create an endpoint gateway for {{site.data.keyword.logs_routing_full_notm}}, follow these steps to connect to {{site.data.keyword.logs_routing_full_notm}}:

### Using the VPE with the {{site.data.keyword.logs_routing_full_notm}} API
{: #vpe-connection-using-api}
{: api}

After creating an endpoint gateway for the {{site.data.keyword.logs_routing_full_notm}} service, use the service endpoints FQDN `private.us-east.logs-router.cloud.ibm.com` in the URL to access the service.

For example:

```sh
curl -X GET -H "Authorization: Bearer ${IAM_TOKEN}" https://management.private.us-east.logs-router.cloud.ibm.com:443/v1/tenants
```
{: pre}


### More resources
{: #vpe-connection-other-resources}

- [Planning for virtual private endpoint gateways](/docs/vpc?topic=vpc-vpe-planning-considerations)
- [VPE connectivity patterns](/docs/vpc?topic=vpc-about-vpe#vpe-connectivity-patterns)
- [Creating an endpoint gateway](/docs/vpc?topic=vpc-ordering-endpoint-gateway)
- For further assistance, see the [FAQ for virtual private endpoints](/docs/vpc?topic=vpc-faqs-vpe).
- For troubleshooting VPE gateways, see other documentation  such as

    - [How to fix communications issues here](/docs/vpc?topic=vpc-troubleshoot-cannot-communicate).

    - [Why can't my endpoint gateway reach the target IBM Cloud service?](/docs/vpc?topic=vpc-troubleshoot-cannot-reach-target)

    - [Accessing your virtual private endpoint after setting up your endpoint gateway](/docs/vpc?topic=vpc-accessing-vpe-after-setup)
