---

copyright:
  years:  2023, 2024
lastupdated: "2024-09-10"

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

## Before you begin
{: #target-update-prereqs}

Complete the following steps:

- Review [About {{site.data.keyword.logs_routing_full_notm}}](/docs/logs-router?topic=logs-router-about) to understand concepts.

- Install all prerequisite tools as described in the [getting started](/docs/logs-router?topic=logs-router-getting-started&interface=ui#getting-started-before-you-begin-2).

- Set up permissions to manage targets in the account. For more information, see [Setting up IAM permissions for managing tenants](/docs/logs-router?topic=logs-router-tenant-iam-permissions).

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

## Updating a target by using the API
{: #target-update-api}
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
    "port": LOG_SINK_INGESTION_PORT,
    "access_credential": "LOG_SINK_INGESTION_KEY"
    }
  }'
```
{: codeblock}

Where

- `<MANAGEMENT-API-ENDPOINT>` is the {{site.data.keyword.logs_routing_full}} endpoint in the region where you plan to collect logs. For more information, see [Endpoints](/docs/logs-router?topic=logs-router-endpoints).
- `LOG_SINK_INGESTION_KEY` is the ingestion key of the target {{site.data.keyword.la_full_notm}} instance.
- `LOG_SINK_CRN` is the CRN of the target {{site.data.keyword.la_full_notm}} instance.
- `LOG_SINK_INGESTION_HOST` is the host of the ingestion endpoint for the log-sink. You can choose a public or a private ingestion endpoint. For more information, see [{{site.data.keyword.la_full_notm}} ingestion endpoints](/docs/log-analysis?topic=log-analysis-endpoints#endpoints_ingestion_public).
- `LOG_SINK_INGESTION_PORT` is the port of the ingestion endpoint for the log-sink.
- `TENANT_ID` is the ID of the tenant that you need to modify.
- `TARGET_ID` is the ID of the target log sink that you need to modify.
- `CURRENT_DATE` is the current date in format YYYY-MM-DD.
- `E_TAG` of target is provided by GET, POST or PATCH.
- `LOG_SINK_NAME` is the new name of the target log sink.

The following example shows updating an {{site.data.keyword.logs_routing_full_notm}} target in the `us-east` region by using a VPE.
In this case, the request changes the target from one {{site.data.keyword.la_short}} instance to a different one, so the fields to be updated are `access_credential` and `log_sink_crn`.
Both instances are in the same `us-east` region:



```sh
curl -X PATCH  https://management.private.us-east.logs-router.cloud.ibm.com:443/v1/tenants/97543c-77b7-eg23-8114-999b31a2bc \
-H "Content-Type: application/merge-patch+json" \
-H "Authorization: ${IAM_TOKEN}" \
-H 'IBM-API-Version: 2024-03-01' \
-H 'If-Match: "8f331e16860324d04ed5c7c90fa076b725a1f440fd19e2885cdc930ff8366f2a"' \
--data "{
    \"access_credential\": \"INGESTIONKEY_FOR_OTHER_INSTANCE\",
    \"log_sink_crn\": \"crn:v1:bluemix:public:logdna:us-east:a/473958g47b35f95747:37a479b-23gc-b874-0f1f-d0f64a61a2bc::\"
    }"
```
{: pre}

A successful request returns a response that contains the updated target, for example:

```json
{
  "name": "my-log-sink",
  "id": "97543c-77b7-eg23-8114-999b31a2bc",
  "type": "logdna",
  "log_sink_crn": "crn:v1:bluemix:public:logdna:us-east:a/473958g47b35f95747:37a479b-23gc-b874-0f1f-d0f64a61a2bc::",
  "created_at": "2020-09-28T17:49+0000",
  "updated_at": "2023-10-23T12:26+0000",
  "etag": "\"8f331e16860324d04ed5c7c90fa076b725a1f440fd19e2885cdc930ff8366f2a\"",
  "parameters": {
      "host": "logs.us-east.logging.cloud.ibm.com",
      "port": 443
  }
}
```
{: screen}

For security reasons, you cannot retrieve an {{site.data.keyword.la_full_notm}} ingestion key that is stored in {{site.data.keyword.logs_routing_full_notm}}. This field is always omitted from API responses, even if you change the ingestion key with an update request.
{: important}
