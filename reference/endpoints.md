---

copyright:
  years:  2022, 2024
lastupdated: "2024-04-29"

keywords:

subcollection: logs-router

---

{{site.data.keyword.attribute-definition-list}}

# Endpoints
{: #endpoints}

Access {{site.data.keyword.logs_routing_full_notm}} by using the listed endpoints for each supported {{site.data.keyword.cloud_notm}} location.
{: shortdesc}

## Public Endpoints
{: #public-endpoints}

Only the management API supports public endpoints, which can be accessed from the public internet. Public access is not available to the ingestion API.

| Geography | Region                           | Management Endpoint | Ingestion Endpint |
|-----------|----------------------------------|---------------------|--------------------|
| Asia Pacific | Osaka (`jp-osa`) | `management.jp-osa.logs-router.cloud.ibm.com` | NA |
| Asia Pacific | Sydney (`au-syd`) | `management.au-syd.logs-router.cloud.ibm.com` | NA |
| Asia Pacific | Tokyo (`jp-tok`) | `management.jp-tok.logs-router.cloud.ibm.com` | NA |
| Europe  | Frankfurt (`eu-de`) | `management.eu-de.logs-router.cloud.ibm.com` | NA |
| Europe  | London (`eu-gb`) | `management.eu-gb.logs-router.cloud.ibm.com` | NA |
| Europe  | Madrid (`eu-es`) | `management.eu-es.logs-router.cloud.ibm.com` | NA |
| North America  | Dallas (`us-south`) | `management.us-south.logs-router.cloud.ibm.com` | NA |
| North America  | Toronto (`ca-tor`) | `management.ca-tor.logs-router.cloud.ibm.com` | NA |
| North America  | Washington, D.C (`us-east`) | `management.us-east.logs-router.cloud.ibm.com` | NA |
| South America  | Sao Paulo (`br-sao`) | `management.br-sao.logs-router.cloud.ibm.com` | NA |
{: caption="Table 1. List of {{site.data.keyword.logs_routing_full_notm}} public endpoints" caption-side="top"}

## Private Endpoints
{: #private-endpoints}

Private endpoints restrict access to the {{site.data.keyword.cloud_notm}} private network.  Both the management and ingestion APIs support private endpoints.

| Geography | Region                           | Management Endpoint | Ingestion Endpint |
|-----------|----------------------------------|---------------------|--------------------|
| Asia Pacific  | Osaka (`jp-osa`) | `management.private.jp-osa.logs-router.cloud.ibm.com` | `ingester.private.jp-osa.logs-router.cloud.ibm.com` |
| Asia Pacific  | Sydney (`au-syd`) | `management.private.au-syd.logs-router.cloud.ibm.com` | `ingester.private.au-syd.logs-router.cloud.ibm.com` |
| Asia Pacific  | Tokyo (`jp-tok`) | `management.private.jp-tok.logs-router.cloud.ibm.com` | `ingester.private.jp-tok.logs-router.cloud.ibm.com` |
| Europe  | Frankfurt (`eu-de`) | `management.private.eu-de.logs-router.cloud.ibm.com` | `ingester.private.eu-de.logs-router.cloud.ibm.com` |
| London  | London (`eu-gb`) | `management.private.eu-gb.logs-router.cloud.ibm.com` | `ingester.private.eu-gb.logs-router.cloud.ibm.com` |
| Europe  | Madrid (`eu-es`) | `management.private.eu-es.logs-router.cloud.ibm.com` | `ingester.private.eu-es.logs-router.cloud.ibm.com` |
| North America  | Dallas (`us-south`) | `management.private.us-south.logs-router.cloud.ibm.com` | `ingester.private.us-south.logs-router.cloud.ibm.com` |
| North America  | Toronto (`ca-tor`) | `management.private.ca-tor.logs-router.cloud.ibm.com` | `ingester.private.ca-tor.logs-router.cloud.ibm.com` |
| North America  | Washington, D.C (`us-east`) | `management.private.us-east.logs-router.cloud.ibm.com` | `ingester.private.us-east.logs-router.cloud.ibm.com` |
| South America  | Sao Paulo (`br-sao`) | `management.private.br-sao.logs-router.cloud.ibm.com` | `ingester.private.br-sao.logs-router.cloud.ibm.com` |
{: caption="Table 2. List of {{site.data.keyword.logs_routing_full_notm}} private endpoints" caption-side="top"}


When you connect by using a [VPE](/docs/vpc?topic=vpc-about-vpe), use port 443 with both management and ingestion endpoints. When you connect by using a [CSE](/docs/account?topic=account-service-endpoints-overview), use port 3443 for both endpoints.
{: important}

For example, to access the `us-east` management endpoint over a CSE, connect to `https://management.private.us-east.logs-router.cloud.ibm.com:3443`. To access the `us-east` ingestion endpoint over a VPE, you would configure the agent to use `https://ingester.private.us-east.logs-router.cloud.ibm.com:443`.

