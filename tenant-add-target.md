---

copyright:
  years:  2023, 2024
lastupdated: "2024-09-12"

keywords:

subcollection: logs-router

---

{{site.data.keyword.attribute-definition-list}}

# Adding a target to a tenant by using the API
{: #tenant-add-target}

You can create and add a second target to a tenant in {{site.data.keyword.logs_routing_full}}.
{: shortdesc}

If you are migrating from {{site.data.keyword.la_full_notm}} to {{site.data.keyword.logs_full_notm}}, you can add the second target to collect platform logs in both the {{site.data.keyword.la_full_notm}} instance and the {{site.data.keyword.logs_full_notm}} instance.
{: tip}

To add a target, you must supply information about the destination where you want your logs delivered. For more information, see [{{site.data.keyword.logs_full_notm}} target destination data](/docs/logs-router?topic=logs-router-tenant-create&interface=api#tenant-create-api-logs) or [{{site.data.keyword.la_full_notm}} target destination data](/docs/logs-router?topic=logs-router-tenant-create&interface=api#tenant-create-api-la).





## Before you begin
{: #tenant-add-target-prereqs}

Complete the following steps:

- Review [About {{site.data.keyword.logs_routing_full}}](/docs/logs-router?topic=logs-router-about) to understand concepts.

- Install all prerequisite tools as described in the [getting started](/docs/logs-router?topic=logs-router-getting-started&interface=ui#getting-started-before-you-begin).

- Set up permissions to manage targets in the account. For more information, see [Setting up IAM permissions for managing tenants](/docs/logs-router?topic=logs-router-iam&interface=ui).

- To get details on a tenant by using the API, check that you can connect to {{site.data.keyword.logs_routing_full_notm}} by using the management API. For more information, see [Connecting to {{site.data.keyword.logs_routing_full}}](/docs/logs-router?topic=logs-router-about#about_connecting).

## Retrieving the IAM bearer token
{: #tenant-add-target-retrieve-iam-token-cli}

You must get an {{site.data.keyword.iamlong}} (IAM) access token to authenticate your requests to the {{site.data.keyword.logs_routing_full}} service. For more information, see [Retrieving an access token](/docs/logs-router?topic=logs-router-retrieve-access-token).

For example, you can retrieve your IAM bearer token and export it as an environment variable by running the following CLI command:

```sh
export IAM_TOKEN=`ibmcloud iam oauth-tokens --output json | jq -r '.iam_token'`
```
{: pre}

## Getting the tenant ID
{: #tenant-add-details}

To get the tenant ID, see [Retrieving tenant information in IBM Cloud Logs Routing](/docs/logs-router?topic=logs-router-tenant-get).



## Choosing the management endpoint
{: #tenant-add-target-endpoint}


A tenant is the account-specific configuration of {{site.data.keyword.logs_routing_full_notm}} running within a region.

To get the details of a tenant in a region, you must use the management endpoint URL for the region where the tenant is configured.
{: important}

You can use private or public endpoints.

For more information, see [Management endpoint URLs](/docs/logs-router?topic=logs-router-endpoints).


## Add a target
{: #tenant-add-target-1}
{: api}

Run the following command to create a target and add the target to a tenant by using the **private endpoint**:

```sh
curl -X POST  https://management.private.${REGION}.logs-router.cloud.ibm.com/v1/tenants/<TENANT-ID>/targets \
-H "Content-Type: application/json" \
-H "Authorization: ${IAM_TOKEN}" \
-H "IBM-API-Version: API_VERSION_DATE" \
--data '{
        "log_sink_crn": "LOG_SINK_CRN",
        "name": "TARGET_NAME",
        "parameters": {
            "host": "LOG_SINK_INGESTION_ENDPOINT",
            "port": LOG_SINK_PORT
        }'
```
{: pre}

Run the following command to create a target and add the target to a tenant by using the **public endpoint**:

```sh
curl -X POST  https://management.${REGION}.logs-router.cloud.ibm.com/v1/tenants<TENANT-ID>/targets \
--H "Content-Type: application/json" \
-H "Authorization: ${IAM_TOKEN}" \
-H "IBM-API-Version: API_VERSION_DATE" \
--data '{
        "log_sink_crn": "LOG_SINK_CRN",
        "name": "TARGET_NAME",
        "parameters": {
            "host": "LOG_SINK_INGESTION_ENDPOINT",
            "port": LOG_SINK_PORT
        }'
```
{: pre}

Where
- `REGION` defines the location where the tenant is configured.
- `IAM_TOKEN` defines the credentials that you use to authenticate your requests.
- `API_VERSION_DATE` defines the date of the API version that you want to use to query your tenant definition. The format must be as follows: `YYYY-MM-DD`
- `TARGET_DATA` defines the information about the target destination.
- `TENANT-ID`: The ID of your existing tenant where you want to add a target.
- `LOG_SINK_CRN`: CRN of the target {{site.data.keyword.logs_full_notm}} instance.
- `LOG_SINK_INGESTION_ENDPOINT`: Full qualified ingestion endpoint for the log-sink.
- `LOG_SINK_PORT`: The corresponding port to the LOG_SINK_INGESTION_ENDPOINT.


The following shows an example of adding a target in us-east by using the public endpoint and an existing tenant with ID 97543c-77b7-eg23-8114-999b31a2b3:

curl -X POST "https://management.us-east.logs-router.cloud.ibm.com/v1/tenants/97543c-77b7-eg23-8114-999b31a2b3/targets" \
--header "Authorization: Bearer TOKEN" \
--header 'Content-Type: application/json' \
--header 'IBM-API-Version: 2024-03-01' \
--data '{
            "log_sink_crn": "crn:v1:bluemix:public:logs:us-east:a/3516b8fa0a174a71899f5affa4f18d78:3517d2ed-9429-af34-ad52-34278391cbc8::",
            "name": "my-log-sink",
            "parameters": {
                "host": "743e3b4f-72bc-4669-9fa6-0e6621d0b232.ingress.eu-es.logs.cloud.ibm.com",
                "port": 443
                }
        }'

{: pre}

If the creation request was successful, a response that contains your tenant metadata is returned.
