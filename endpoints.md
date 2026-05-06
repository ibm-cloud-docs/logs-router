---

copyright:
  years:  2023, 2026
lastupdated: "2026-05-06"

keywords:

subcollection: logs-router

---

{{site.data.keyword.attribute-definition-list}}

# Endpoints
{: #endpoints}

Access {{site.data.keyword.logs_routing_full_notm}} by using the listed endpoints for each supported {{site.data.keyword.cloud_notm}} location.
{: shortdesc}


## Public Endpoints
{: #public_endpoints_v1}

The management API supports public endpoints, which can be accessed from the public internet. 

| Geography | Region                           | Management Endpoint | Ingestion Endpoint |
|-----------|----------------------------------|---------------------|--------------------|
| Asia Pacific | Chennai (`in-che`) | `management.in-che.logs-router.cloud.ibm.com` | NA |
| Asia Pacific | Mumbai (`in-mum`) | `management.in-mum.logs-router.cloud.ibm.com` | NA |
| Asia Pacific | Osaka (`jp-osa`) | `management.jp-osa.logs-router.cloud.ibm.com` | NA |
| Asia Pacific | Sydney (`au-syd`) | `management.au-syd.logs-router.cloud.ibm.com` | NA |
| Asia Pacific | Tokyo (`jp-tok`) | `management.jp-tok.logs-router.cloud.ibm.com` | NA |
| Europe  | Frankfurt (`eu-de`) | `management.eu-de.logs-router.cloud.ibm.com` | NA |
| Europe  | London (`eu-gb`) | `management.eu-gb.logs-router.cloud.ibm.com` | NA |
| Europe  | Madrid (`eu-es`) | `management.eu-es.logs-router.cloud.ibm.com` | NA |
| North America  | Dallas (`us-south`) | `management.us-south.logs-router.cloud.ibm.com` | NA |
| North America  | Montreal (`ca-mon`) | `management.ca-mon.logs-router.cloud.ibm.com` | NA |
| North America  | Toronto (`ca-tor`) | `management.ca-tor.logs-router.cloud.ibm.com` | NA |
| North America  | Washington, D.C (`us-east`) | `management.us-east.logs-router.cloud.ibm.com` | NA |
| South America  | Sao Paulo (`br-sao`) | `management.br-sao.logs-router.cloud.ibm.com` | NA |
{: caption="List of {{site.data.keyword.logs_routing_full_notm}} public endpoints" caption-side="top"}

## Private Endpoints
{: #private_endpoints_v1}

Private endpoints restrict access to the {{site.data.keyword.cloud_notm}} private network.  







When you connect by using a [VPE](/docs/vpc?topic=vpc-about-vpe), use port 443. When you connect by using a [CSE](/docs/account?topic=account-service-endpoints-overview), use port 3443.
{: important}

For example, to access the `us-east` management endpoint over a CSE, connect to `https://management.private.us-east.logs-router.cloud.ibm.com:3443`. 
