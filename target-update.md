---

copyright:
  years:  2023, 2025
lastupdated: "2025-05-01"

keywords:

subcollection: logs-router

---

{{site.data.keyword.attribute-definition-list}}

# Updating the configuration details of a target in {{site.data.keyword.logs_routing_full_notm}}
{: #target-update}

You can update the configuration details of a target in {{site.data.keyword.logs_routing_full_notm}}.
{: shortdesc}

{{site.data.content.tenant_definition_note}}

A change of the target type is only supported for tenants with a single target.
{: important}

## Before you begin
{: #target-update-prereqs}

Complete the following steps:

- Review [About {{site.data.keyword.logs_routing_full_notm}}](/docs/logs-router?topic=logs-router-about) to understand concepts.

- Install all prerequisite tools as described in the [getting started](/docs/logs-router?topic=logs-router-getting-started&interface=ui#getting-started-before-you-begin).

- Set up permissions to manage targets in the account. For more information, see [Setting up IAM permissions for managing tenants](/docs/logs-router?topic=logs-router-iam&interface=ui).

- To get details on a tenant by using the API, check that you can connect to {{site.data.keyword.logs_routing_full_notm}} by using the management API. For more information, see [Connecting to {{site.data.keyword.logs_routing_full}}](/docs/logs-router?topic=logs-router-about#about_connecting).



## Retrieving the IAM bearer token
{: #target-update-retrieve-iam-token}


You must get an {{site.data.keyword.iamlong}} (IAM) access token to authenticate your requests to the {{site.data.keyword.logs_routing_full}} service. For more information, see [Retrieving an access token](/docs/logs-router?topic=logs-router-retrieve-access-token).

For example, you can retrieve your IAM bearer token and export it as an environment variable by running the following CLI command:

```sh
export IAM_TOKEN=`ibmcloud iam oauth-tokens --output json | jq -r '.iam_token'`
```
{: pre}



## Getting the tenant ID
{: #target-update-details}

To get the tenant ID and target current details, see [Retrieving tenant information in IBM Cloud Logs Routing](/docs/logs-router?topic=logs-router-tenant-get).


## Choosing the management endpoint
{: #target-update-endpoint}


A tenant is the account-specific configuration of {{site.data.keyword.logs_routing_full_notm}} running within a region.

To get the details of a tenant in a region, you must use the management endpoint URL for the region where the tenant is configured.
{: important}

You can use private or public endpoints.

For more information, see [Management endpoint URLs](/docs/logs-router?topic=logs-router-endpoints).



## Updating an {{site.data.keyword.logs_full_notm}} target by using the API
{: #target-update-api-cl}
{: api}

In the request body, you are only required to supply the fields you are modifying. Any fields that are not included in the request body are not changed.
{: note}

```shell
curl -X PATCH "https://<MANAGEMENT-API-ENDPOINT>/v1/tenants/TENANT_ID/targets/TARGET_ID" \
--header 'Authorization: ${IAM_TOKEN}' \
--header 'Content-Type: application/merge-patch+json' \
--header 'IBM-API-Version: CURRENT_DATE' \
--header 'If-Match: E_TAG' \
--data '{
  "log_sink_crn":"LOG_SINK_CRN",
  "name": "LOG_SINK_NAME",
  "parameters": {
    "host": "LOG_SINK_INGESTION_HOST",
    "port": LOG_SINK_INGESTION_PORT
    }
  }'
```
{: codeblock}

Where

- `<MANAGEMENT-API-ENDPOINT>` is the {{site.data.keyword.logs_routing_full}} endpoint in the region where you plan to collect logs. For more information, see [Endpoints](/docs/logs-router?topic=logs-router-endpoints).
- `LOG_SINK_CRN` is the CRN of the {{site.data.keyword.logs_full_notm}} instance.
- `LOG_SINK_INGESTION_HOST` is the host of the ingestion endpoint for the log-sink. You can choose a public or a private ingestion endpoint. For more information, see [Ingress endpoints](/docs/cloud-logs?topic=cloud-logs-endpoints_ingress).
- `LOG_SINK_INGESTION_PORT` is the port of the ingestion endpoint for the log-sink.
- `TENANT_ID` is the ID of the tenant that you need to modify.
- `TARGET_ID` is the ID of the target log sink that you need to modify.
- `CURRENT_DATE` is the current date in format YYYY-MM-DD.
- `E_TAG` of target. You must [retrieve the tenant information](/docs/logs-router?topic=logs-router-tenant-get) to get the latest `e_tag` associated with a target before making an update to the target.

    Every time that you update a target, the `e_tag` of the target changes.
    {: note}

- `LOG_SINK_NAME` is the new name of the target log sink.
