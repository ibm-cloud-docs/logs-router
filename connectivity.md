---

copyright:
  years:  2023, 2024
lastupdated: "2024-08-24"

keywords:

subcollection: logs-router

---

{{site.data.keyword.attribute-definition-list}}

# Connectivity to manage the {{site.data.keyword.logs_routing_full}} service
{: #connectivity}

You can manage {{site.data.keyword.logs_routing_full_notm}} by using the management API. The management API supports either a public endpoint or a private endpoint. A public endpoint can be reached over the internet, whereas a private endpoint can be accessed only from within the {{site.data.keyword.cloud_notm}} private network.
{: shortdesc}


- To create, update, or delete a configuration, you must use the {{site.data.keyword.logs_routing_full_notm}} management API.
- To see the list of management endpoints, see [Endpoints](/docs/logs-router?topic=logs-router-endpoints).


## Using public endpoints
{: #connectivity-public}

Using the public endpoint does not require additional work.


## Using private endpoints
{: #connectivity-private}

The {{site.data.keyword.logs_routing_full}} supports the following types of endpoints to privately connect to {{site.data.keyword.logs_routing_full_notm}}:
- {{site.data.keyword.cloud_notm}} Cloud Service Endpoint (CSE)
- {{site.data.keyword.vpe_full}} for VPC.

Choose one of the following options to privately connect to {{site.data.keyword.logs_routing_full_notm}}:
- [Using virtual private endpoints for VPC to privately connect to {{site.data.keyword.logs_routing_full_notm}}](/docs/logs-router?topic=logs-router-vpe-connection).
- [Using service endpoints to privately connect to {{site.data.keyword.logs_routing_full_notm}}](/docs/logs-router?topic=logs-router-service-endpoints).
