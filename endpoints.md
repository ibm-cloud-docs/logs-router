---

copyright:
  years:  2023, 2026
lastupdated: "2026-04-28"

keywords:

subcollection: logs-router

---

{{site.data.keyword.attribute-definition-list}}

# Endpoints
{: #endpoints}

Access {{site.data.keyword.logs_routing_full_notm}} by using the listed endpoints for each supported {{site.data.keyword.cloud_notm}} location.
{: shortdesc}

## {{site.data.keyword.logs_routing_full_notm}} V1
{: #endpoints_v1}

### Public Endpoints
{: #public_endpoints_v1}

The management API supports public endpoints, which can be accessed from the public internet. 

| Geography | Region                           | Management Endpoint | Ingestion Endpoint |
|-----------|----------------------------------|---------------------|--------------------|
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

### Private Endpoints
{: #private_endpoints_v1}

Private endpoints restrict access to the {{site.data.keyword.cloud_notm}} private network.  







When you connect by using a [VPE](/docs/vpc?topic=vpc-about-vpe), use port 443. When you connect by using a [CSE](/docs/account?topic=account-service-endpoints-overview), use port 3443.
{: important}

For example, to access the `us-east` management endpoint over a CSE, connect to `https://management.private.us-east.logs-router.cloud.ibm.com:3443`. 



## {{site.data.keyword.logs_routing_full_notm}} V3
{: #endpoints_api_v3}

The IP addresses with a date next to them indicate when the IP addresses will be available. IP addresses marked as [Deprecated]{: tag-deprecated} and a date indicate when the IP addresses will no longer be supported and should be removed from any configured allowlists.
{: important}

### Private API endpoints
{: #endpoints_api_v3_private}

The following table shows the private API endpoints. The port for all endpoints is `https/443` .

| Region                   | ATracker Private endpoint                         | IPs |
|--------------------------|---------------------------------------------------|-------|
| Chennai (`in-che`)      | `https://api.private.in-che.logs-router.cloud.ibm.com` | 166.9.249.118  \n 166.9.249.147  \n 166.9.249.183 |
| Dallas (`us-south`)      | `https://api.private.us-south.logs-router.cloud.ibm.com` | 166.9.228.73   \n 166.9.229.64   \n 166.9.230.57  |
| Frankfurt (`eu-de`)      | `https://api.private.eu-de.logs-router.cloud.ibm.com`  |  166.9.209.204   \n 166.9.209.236   \n 166.9.210.2  |
| London (`eu-gb`)         | `https://api.private.eu-gb.logs-router.cloud.ibm.com`  |  166.9.245.170   \n 166.9.245.202   \n 166.9.245.234  |
| Madrid (`eu-es`)         | `https://api.private.eu-es.logs-router.cloud.ibm.com`  | 166.9.225.11  \n 166.9.226.12  \n 166.9.227.11 |
| Montreal (`ca-mon`)         | `https://api.private.ca-mon.logs-router.cloud.ibm.com`  | 166.9.232.49  \n 166.9.231.29  \n 166.9.233.49  |
| Osaka (`jp-osa`)         | `https://api.private.jp-osa.logs-router.cloud.ibm.com`  | 166.9.247.46  \n 166.9.247.71  \n 166.9.247.110 |
| Sao Paulo (`br-sao`)        | `https://api.private.br-sao.logs-router.cloud.ibm.com` | 166.9.246.76  \n 166.9.246.109  \n 166.9.246.157 |
| Sydney (`au-syd`)        | `https://api.private.au-syd.logs-router.cloud.ibm.com` |  166.9.244.119   \n 166.9.244.151   \n 166.9.244.183  |
| Tokyo  (`jp-tok`)         | `https://api.private.jp-tok.logs-router.cloud.ibm.com`  | 166.9.249.115   \n 166.9.249.144  \n 166.9.249.180 |
| Toronto  (`ca-tor`)         | `https://api.private.ca-tor.logs-router.cloud.ibm.com`  | 166.9.247.154   \n 166.9.247.183  \n 166.9.247.216 |
| Washington (`us-east`)   | `https://api.private.us-east.logs-router.cloud.ibm.com`  |  166.9.231.252   \n 166.9.251.89   \n 166.9.233.28  |
{: caption="Lists of private API endpoints for interacting with {{site.data.keyword.logs-router_full_notm}}" caption-side="top"}


### Public API endpoints
{: #endpoints_api_v3_public}

The following table shows the public API endpoints:

| Region                   | ATracker Public endpoint                         | Port         |
|--------------------------|---------------------------------------------------|--------------|
| Chennai (`in-che`)       | `https://api.in-che.logs-router.cloud.ibm.com`         | `https/443`  |
| Dallas (`us-south`)      | `https://api.us-south.logs-router.cloud.ibm.com`         | `https/443`  |
| Frankfurt (`eu-de`)      | `https://api.eu-de.logs-router.cloud.ibm.com`          | `https/443`  |
| London (`eu-gb`)         | `https://api.eu-gb.logs-router.cloud.ibm.com`          | `https/443`  |
| Madrid (`eu-es`)         | `https://api.eu-es.logs-router.cloud.ibm.com`          | `https/443`  |
| Montreal (`ca-mon`)      | `https://api.ca-mon.logs-router.cloud.ibm.com`          | `https/443`  |
| Osaka (`jp-osa`)         | `https://api.jp-osa.logs-router.cloud.ibm.com`          | `https/443`  |
| Sao Paulo (`br-sao`)     | `https://api.br-sao.logs-router.cloud.ibm.com`          | `https/443`  |
| Sydney (`au-syd`)        | `https://api.au-syd.logs-router.cloud.ibm.com`          | `https/443`  |
| Tokyo (`jp-tok`)         | `https://api.jp-tok.logs-router.cloud.ibm.com`          | `https/443`  |
| Toronto (`ca-tor`)       | `https://api.ca-tor.logs-router.cloud.ibm.com`          | `https/443`  |
| Washington (`us-east`)   | `https://api.us-east.logs-router.cloud.ibm.com`          | `https/443`  |
{: caption="Lists of public API endpoints for interacting with {{site.data.keyword.logs-router_full_notm}}" caption-side="top"}
