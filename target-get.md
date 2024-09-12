---

copyright:
  years:  2023, 2024
lastupdated: "2024-09-12"

keywords:

subcollection: logs-router

---

{{site.data.keyword.attribute-definition-list}}

# Retrieving target information
{: #target-get}

You can get information for a target that is configured for a tenant in {{site.data.keyword.logs_routing_full}} by using the tenant ID and the target ID.
{: shortdesc}

{{site.data.content.tenant_definition_note}}


## Before you begin
{: #target-get-prereqs}

Complete the following steps:

- Review [About {{site.data.keyword.logs_routing_full_notm}}](/docs/logs-router?topic=logs-router-about) to understand concepts.

- Install all prerequisite tools as described in the [getting started](/docs/logs-router?topic=logs-router-getting-started&interface=ui#getting-started-before-you-begin).

- Set up permissions to manage targets in the account. For more information, see [Setting up IAM permissions for managing tenants](/docs/logs-router?topic=logs-router-iam&interface=ui).

- To get details on a tenant by using the API, check that you can connect to {{site.data.keyword.logs_routing_full_notm}} by using the management API. For more information, see [Connecting to {{site.data.keyword.logs_routing_full}}](/docs/logs-router?topic=logs-router-about#about_connecting).



## Retrieving the IAM bearer token
{: #target-get-retrieve-iam-token}


You must get an {{site.data.keyword.iamlong}} (IAM) access token to authenticate your requests to the {{site.data.keyword.logs_routing_full}} service. For more information, see [Retrieving an access token](/docs/logs-router?topic=logs-router-retrieve-access-token).

For example, you can retrieve your IAM bearer token and export it as an environment variable by running the following CLI command:

```sh
export IAM_TOKEN=`ibmcloud iam oauth-tokens --output json | jq -r '.iam_token'`
```
{: pre}



## Getting the tenant ID
{: #target-get-details}

To get the tenant ID, see [Retrieving tenant information in IBM Cloud Logs Routing](/docs/logs-router?topic=logs-router-tenant-get).

## Choosing the management endpoint
{: #target-get-endpoint}


A tenant is the account-specific configuration of {{site.data.keyword.logs_routing_full_notm}} running within a region.

To get the details of a tenant in a region, you must use the management endpoint URL for the region where the tenant is configured.
{: important}

You can use private or public endpoints.

For more information, see [Management endpoint URLs](/docs/logs-router?topic=logs-router-endpoints).



## Getting target information by using the API
{: #target-get-api}
{: api}

Run the following command to get the details of a target from a tenant by using the **private endpoint**:

```sh
curl -X GET  https://management.private.${REGION}.logs-router.cloud.ibm.com/v1/tenants/${TENANT_ID}/targets/<TARGET_ID> \
-H "IBM-API-Version: API_VERSION_DATE" \
-H "Authorization: ${IAM_TOKEN}"
```
{: pre}


Run the following command to get the details of a target from a tenant by using the **public endpoint**:

```sh
curl -X GET  https://management.${REGION}.logs-router.cloud.ibm.com/v1/tenants/${TENANT_ID}/targets/<TARGET_ID> \
-H "IBM-API-Version: API_VERSION_DATE" \
-H "Authorization: ${IAM_TOKEN}"
```
{: pre}

Where:

- `IAM_TOKEN` is the IAM token that you must use to authenticate the request.

- `TENANT_ID` defines the ID of the tenant that you are removing which was returned when the service was created.

- `REGION` defines the region where your tenant is located.

- `TARGET_ID` is the ID of the target that you want to get the details.

- `API_VERSION_DATE` defines the date of the API version that you want to use to query your tenant definition. The format must be as follows: `YYYY-MM-DD`


For example, you can run the following sample to get the details of a target configured in a tenant located in `us-east`:


```sh
curl -X GET https://management.private.us-east.logs-router.cloud.ibm.com:443/v1/tenants/97543c-77b7-eg23-8114-999b31a2b3 \
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
  ]
}
```
{: screen}
