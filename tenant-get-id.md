---

copyright:
  years:  2023, 2025
lastupdated: "2025-01-23"

keywords:

subcollection: logs-router

---

{{site.data.keyword.attribute-definition-list}}

# Retrieving tenant information in {{site.data.keyword.logs_routing_full_notm}} by using the tenant ID
{: #tenant-get-id}

You can get information for an existing tenant that you define in {{site.data.keyword.logs_routing_full}} by using the tenant ID.
{: shortdesc}

{{site.data.content.tenant_definition_note}}


## Before you begin
{: #tenant-get-id-prereqs}

Complete the following steps:

1. Review [About {{site.data.keyword.logs_routing_full}}](/docs/logs-router?topic=logs-router-about) to understand concepts.

2. Install all prerequisite tools as described in the [getting started](/docs/logs-router?topic=logs-router-getting-started&interface=ui#getting-started-before-you-begin).

3. Set up permissions to manage targets in the account. For more information, see [Setting up IAM permissions for managing tenants](/docs/logs-router?topic=logs-router-iam&interface=ui).

4. To get details on a tenant by using the API, check that you can connect to {{site.data.keyword.logs_routing_full_notm}} by using the management API. For more information, see [Connecting to {{site.data.keyword.logs_routing_full}}](/docs/logs-router?topic=logs-router-about#about_connecting).

## Getting the IAM bearer token
{: #tenant-get-id-iam-token}


You must get an {{site.data.keyword.iamlong}} (IAM) access token to authenticate your requests to the {{site.data.keyword.logs_routing_full}} service. For more information, see [Retrieving an access token](/docs/logs-router?topic=logs-router-retrieve-access-token).

For example, you can retrieve your IAM bearer token and export it as an environment variable by running the following CLI command:

```sh
export IAM_TOKEN=`ibmcloud iam oauth-tokens --output json | jq -r '.iam_token'`
```
{: pre}


## Choosing the management endpoint
{: #tenant-get-id-endpoint}


A tenant is the account-specific configuration of {{site.data.keyword.logs_routing_full_notm}} running within a region.

To get the details of a tenant in a region, you must use the management endpoint URL for the region where the tenant is configured.
{: important}

You can use private or public endpoints.

For more information, see [Management endpoint URLs](/docs/logs-router?topic=logs-router-endpoints).



## Retrieving the tenant ID
{: #tenant-get-id-tenant-id}

To get the tenant ID, see [Retrieving tenant information](/docs/logs-router?topic=logs-router-tenant-get).



## Getting tenant information by using the API
{: #tenant-get-id-api}


Run the following command to get the details of a tenant in a region by using the **private endpoint**:

```sh
curl -X GET https://management.private.{REGION}.logs-router.cloud.ibm.com/v1/tenants/${TENANT_ID} \
-H "Authorization: ${IAM_TOKEN}" \
-H "IBM-API-Version: API_VERSION_DATE"
```
{: pre}

Run the following command to get the details of a tenant in a region by using the **public endpoint**:

```sh
curl -X GET https://management.{REGION}.logs-router.cloud.ibm.com/v1/tenants/${TENANT_ID} \
-H "Authorization: ${IAM_TOKEN}" \
-H "IBM-API-Version: API_VERSION_DATE"
```
{: pre}


Where
- `REGION` defines the location where the tenant is configured.
- `IAM_TOKEN` defines the credentials that you use to authenticate your requests.
- `API_VERSION_DATE` defines the current date to request the latest version of the API. The valid format is `YYYY-MM-DD`. Any date up to the current date can be provided.
- `TENANT_ID` defines the ID of the tenant for which you want to get details.


The following example shows how to get information about an {{site.data.keyword.logs_routing_full_notm}} tenant in the `us-east` region by using a VPE:

```sh
curl -X GET https://management.private.us-east.logs-router.cloud.ibm.com/v1/tenants/97543c-77b7-eg23-8114-999b31a2b3 \
-H "Authorization: ${IAM_TOKEN}" \
-H "IBM-API-Version: 2024-03-01"
```
{: pre}

A successful request returns a response that contains a single tenant, for example:

```json
{
  "id": "97543c-77b7-eg23-8114-999b31a2b3",
  "created_at": "2023-10-20T18:30:00.143156Z",
  "updated_at": "2023-10-20T18:30:00.143156Z",
  "crn": "crn:v1:bluemix:public:logs-router:eu-de:a/3516b8fa0a174a71899f5affa4f18d78:3517d2ed-9429-af34-ad52-34278391cbc8::",
  "name": "tenant",
  "etag": "\"822b4b5423e225206c1d75666595714a11925cd0f82b229839864443d6c3c049\"",
  "targets": [
    {
      "id": "86432b-66a6-df12-7003-888a21a2b3",
      "log_sink_crn": "rn:v1:bluemix:public:logdna:us-east:a/473958g47b35f95747:48b580c-34ad-c985-1g2g-e1g75b71a2b3::",
      "name": "my-log-sink",
      "etag": "\"c3a43545a7f2675970671ac3a57b8db067a1866b2222e1b950ee8da612e347c6\"",
      "type": "logdna",
      "created_at": "2023-10-20T18:30:00.143156Z",
      "updated_at": "2023-10-20T18:30:00.143156Z",
      "parameters": {
        "host": "logs.us-east.logging.cloud.ibm.com",
        "port": 443
      }
    }
  ],
  "write_status": {
    "status": "success"
  }
}
```
{: screen}

The `write_status` describes if your tenant is allowed to send logs.
This status is set to `failed`, if the target endpoint provided cannot be reached or ingestion is rejected.
You can find information on how to resolve a failed write status in the [troubleshooting guide for blocked ingestion](/docs/logs-router?topic=logs-router-ts-target-disabled).
