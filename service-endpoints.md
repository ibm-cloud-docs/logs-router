---

copyright:
  years:  2023, 2024
lastupdated: "2024-08-29"

keywords:

subcollection: logs-router

---

{{site.data.keyword.attribute-definition-list}}


# Using service endpoints to privately connect to {{site.data.keyword.logs_routing_full_notm}}
{: #service-endpoints}

To ensure that you have enhanced control and security over your data when you use {{site.data.keyword.logs_routing_full_notm}}, you have the option of using private routes to {{site.data.keyword.cloud}} service endpoints. Private routes are not accessible or reachable over the internet. By using the {{site.data.keyword.cloud_notm}} private service endpoints feature, you can protect your data from threats from the public network and logically extend your private network.
{: shortdesc}


## Before you begin
{: #service-endpoints-prereqs}

You can connect to {{site.data.keyword.logs_routing_full_notm}} over a private network by using {{site.data.keyword.cloud_notm}} private service endpoints (CSE). For more information about CSEs, see the documentation for [using service endpoints](/docs/account?topic=account-service-endpoints-overview).

- When you use the classic infrastructure, you connect to resources in your account over the {{site.data.keyword.cloud_notm}} public network by default. You can enable virtual routing and forwarding (VRF) to move IP routing for your account and all of its resources into a separate routing table. If VRF is enabled, you can then enable {{site.data.keyword.cloud_notm}} service endpoints to connect directly to resources without using the public network. [Enabling VRF and service endpoints](/docs/account?topic=account-vrf-service-endpoint).

- Virtual Private Clouds (VPCs) are automatically enabled for virtual routing and forwarding (VRF). To enable service endpoints for your VPC, continue to [Enabling service endpoints](/docs/account?topic=account-vrf-service-endpoint#service-endpoint).

You must first enable virtual routing and forwarding in your account, and then, you can enable the use of IBM Cloud private service endpoints.
{: note}

### Check if the account is VRF enabled
{: #service-endpoints-prereqs-1}

To check whether the account is VRF enabled, run the following command:

```text
ibmcloud account show
```
{: pre}

### Enable VRF in the account
{: #service-endpoints-prereqs-2}

To enable private endpoints, run the following command:

```text
ibmcloud account update --service-endpoint-enable true
```
{: pre}


## Setting up service endpoints for {{site.data.keyword.logs_routing_full_notm}}
{: #service-endpoints-setup}

You can connect to {{site.data.keyword.logs_routing_full_notm}} management API by using either a public endpoint or a private endpoint. A public endpoint can be reached over the internet, whereas a private endpoint can be accessed only from within the {{site.data.keyword.cloud_notm}} private network.

The ingestion API supports only private endpoints and is therefore not accessible from the public internet.

By default, private and public endpoints are enabled. For more information about supported endpoints, see [Endpoints](/docs/logs-router?topic=logs-router-endpoints).

## Disabling public service endpoints for {{site.data.keyword.logs_routing_full_notm}}
{: #service-endpoints-disable}

You cannot disable public endpoints.

## Disabling private service endpoints
{: #endpoint-disable-private}

You cannot disable private endpoints.
