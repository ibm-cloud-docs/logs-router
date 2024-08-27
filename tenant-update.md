---

copyright:
  years:  2023, 2024
lastupdated: "2024-08-27"

keywords:

subcollection: logs-router

---

{{site.data.keyword.attribute-definition-list}}

# Updating a tenant in {{site.data.keyword.logs_routing_full_notm}}
{: #tenant-update}

After you [create a tenant](/docs/logs-router?topic=logs-router-onboarding) in your account, you can update information about your tenant.
{: shortdesc}

{{site.data.content.tenant_definition_note}}



## Before you begin
{: #tenant-update-prereqs}

Complete the following steps:

1. Review [About {{site.data.keyword.logs_routing_full}}](/docs/logs-router?topic=logs-router-about) to understand concepts.

2. Install all prerequisite tools as described in the [getting started](/docs/logs-router?topic=logs-router-getting-started&interface=ui#getting-started-before-you-begin-2).

3. Set up permissions to manage targets in the account. For more information, see [Setting up IAM permissions for managing tenants](/docs/logs-router?topic=logs-router-tenant-iam-permissions).

4. To get details on a tenant by using the API, check that you can connect to {{site.data.keyword.logs_routing_full_notm}} by using the management API. For more information, see [Connecting to {{site.data.keyword.logs_routing_full}}](/docs/logs-router?topic=logs-router-about#about_connecting).


### Getting the tenant ID
{: #tenant-update-get-id}

Updating a tenant requires the tenant ID. If you do not remember your tenant ID, see [Retrieving tenant information in IBM Cloud Logs Routing](/docs/logs-router?topic=logs-router-tenant-get).
{: important}

### Retrieving {{site.data.keyword.loganalysisfull_notm}} information
{: #retrieve-log-analysis-information}

You can modify only the target for an existing tenant.

{{site.data.keyword.logs_routing_full_notm}} supports targets of type `logdna` ({{site.data.keyword.loganalysisfull_notm}}).

To get the information you need, see [retrieving your {{site.data.keyword.la_full_notm}} information](/docs/logs-router?topic=logs-router-onboarding&interface=ui#onboarding-retrieve-log-analysis-information).


## Retrieving the IAM bearer token
{: #tenant-update-retrieve-iam-token-cli}

You must get an {{site.data.keyword.iamlong}} (IAM) access token to authenticate your requests to the {{site.data.keyword.logs_routing_full}} service. For more information, see [Retrieving an access token](/docs/logs-router?topic=logs-router-retrieve-access-token).

For example, you can retrieve your IAM bearer token and export it as an environment variable by running the following CLI command:

```sh
export IAM_TOKEN=`ibmcloud iam oauth-tokens --output json | jq -r '.iam_token'`
```
{: pre}

## Updating a tenant by using the API
{: #tenant-update-api}
{: api}

Submit the update tenant request to {{site.data.keyword.logs_routing_full_notm}} by using the appropriate [management endpoint URL for the correct region](/docs/logs-router?topic=logs-router-endpoints).

In the request body, you are only required to supply the fields you are modifying. Any fields that are not included in the request body are not changed.
{: note}

```shell
curl -X PATCH "https://<MANAGEMENT-API-ENDPOINT>/v1/tenants/TENANT_ID" \
--header 'Authorization: ${IAM_TOKEN}' \
--header 'Content-Type: application/merge-patch+json' \
--header 'IBM-API-Version: CURRENT_DATE' \
--header 'If-Match: E_TAG' \
--data '{"name":"NAME"}'
```
{: codeblock}

Where

- `<MANAGEMENT-API-ENDPOINT>` is the {{site.data.keyword.logs_routing_full}} endpoint in the region where you plan to collect logs. For more information, see [Endpoints](/docs/logs-router?topic=logs-router-endpoints).
- `TENANT_ID` is the ID of the tenant that you need to modify.
- `CURRENT_DATE` is the current date in format YYYY-MM-DD
- `E_TAG` is provided by GET, POST or PATCH
- `NAME` is the name of the tenant

The following example shows updating an {{site.data.keyword.logs_routing_full_notm}} tenant in the `us-east` region

```shell
curl -X PATCH "https://management.us-east.logs-router.cloud.ibm.com/v1/tenants/8717DB99-2CFB-4BA6-A033-89C994C2E9F0" --header 'Authorization: Bearer TOKEN' --header 'Content-Type: application/merge-patch+json' --header 'IBM-API-Version: 2024-03-01' --header 'If-Match: "\"72f6473f0af26df1f0ae8b993e807ff7ca81b5e7a99d7dbcf4bf4a0b1554fcd2\""' --data '{"name":"a-new-name"}'
```

A successful request returns a response that contains the updated tenant, for example:

```json
{
  "id": "8717DB99-2CFB-4BA6-A033-89C994C2E9F0",
  "created_at": "2023-10-20T18:30:00.143156Z",
  "updated_at": "2023-10-20T18:30:00.143156Z",
  "crn": "crn:v1:bluemix:public:logs-router:eu-de:a/3516b8fa0a174a71899f5affa4f18d78:3517d2ed-9429-af34-ad52-34278391cbc8::",
  "name": "a-new-name",
  "etag": "\"822b4b5423e225206c1d75666595714a11925cd0f82b229839864443d6c3c049\"",
  "targets": [
    {
      "id": "C1C1C838-A4AC-4BD7-8BC6-3173B272429D",
      "log_sink_crn": "crn:v1:bluemix:public:logdna:eu-de:a/3516b8fa0a174a71899f5affa4f18d78:3517d2ed-9429-af34-ad52-34278391cbc8::",
      "name": "my-log-sink",
      "etag": "\"c3a43545a7f2675970671ac3a57b8db067a1866b2222e1b950ee8da612e347c6\"",
      "type": "logdna",
      "created_at": "2023-10-20T18:30:00.143156Z",
      "updated_at": "2023-10-20T18:30:00.143156Z",
      "parameters": {
        "host": "www.example.com",
        "port": 8080
      }
    }
  ]
}
```

## Updating a target by using the API
{: #update-target-api}
{: api}

Submit the update target request to {{site.data.keyword.logs_routing_full_notm}} by using the appropriate [management endpoint URL for the correct region](/docs/logs-router?topic=logs-router-endpoints).

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


## Editing a target through the UI
{: #tenant-update-ui}
{: ui}

When the {{site.data.keyword.logs_routing_full_notm}} console is first displayed, any existing target information is displayed.

To edit a target for a {{site.data.keyword.logs_routing_full_notm}} tenant in a region:

1. Click the ![Actions icon](../icons/action-menu-icon.svg "Actions") next to the region that you want to change.

2. Click **Edit target**.

3. Select an {{site.data.keyword.la_full_notm}} instance to receive logs routed by {{site.data.keyword.logs_routing_full_notm}}.

   You can select an {{site.data.keyword.la_full_notm}} instance by selecting an instance from the list and providing the instance [ingestion key](/docs/log-analysis?topic=log-analysis-ingestion_key) or by specifying an {{site.data.keyword.la_full_notm}} CRN and [ingestion key](/docs/log-analysis?topic=log-analysis-ingestion_key).

   Only {{site.data.keyword.la_full_notm}} instances in your account can be selected by name and ingestion key. If you want to route to an {{site.data.keyword.la_full_notm}} instance in another account, you must select the target by [CRN (Cloud Resource Name)](https://cloud.ibm.com/docs/account?topic=account-crn) and ingestion key. The CRN of an {{site.data.keyword.la_full_notm}} instance can be found by the account administrator of the {{site.data.keyword.la_full_notm}} instance by clicking ![Menu icon](../../icons/icon_hamburger.svg "Menu") > **Resource list** and clicking the {{site.data.keyword.la_full_notm}} instance. The CRN can be copied from the **Details** section.
   {: note}

4. Click **Save**.
