---

copyright:
  years:  2023, 2024
lastupdated: "2024-09-12"

keywords:

subcollection: logs-router

---

{{site.data.keyword.attribute-definition-list}}

# Change the name of a tenant
{: #tenant-update-name}

After you [create a tenant](/docs/logs-router?topic=logs-router-about&interface=ui#about_tenants) in your account, you can change the name of your tenant.
{: shortdesc}

{{site.data.content.tenant_definition_note}}

You cannot change the name of a tenant in the {{site.data.keyword.logs_routing_full_notm}} UI.
{: important}


## Before you begin
{: #tenant-update-name-prereqs}

Complete the following steps:

- Review [About {{site.data.keyword.logs_routing_full_notm}}](/docs/logs-router?topic=logs-router-about) to understand concepts.

- Install all prerequisite tools as described in the [getting started](/docs/logs-router?topic=logs-router-getting-started&interface=ui#getting-started-before-you-begin).

- Set up permissions to manage targets in the account. For more information, see [Setting up IAM permissions for managing tenants](/docs/logs-router?topic=logs-router-iam&interface=ui).

- To get details on a tenant by using the API, check that you can connect to {{site.data.keyword.logs_routing_full_notm}} by using the management API. For more information, see [Connecting to {{site.data.keyword.logs_routing_full}}](/docs/logs-router?topic=logs-router-about#about_connecting).




## Retrieving the IAM bearer token
{: #tenant-update-name-retrieve-iam-token}


You must get an {{site.data.keyword.iamlong}} (IAM) access token to authenticate your requests to the {{site.data.keyword.logs_routing_full}} service. For more information, see [Retrieving an access token](/docs/logs-router?topic=logs-router-retrieve-access-token).

For example, you can retrieve your IAM bearer token and export it as an environment variable by running the following CLI command:

```sh
export IAM_TOKEN=`ibmcloud iam oauth-tokens --output json | jq -r '.iam_token'`
```
{: pre}



## Retrieving the tenant ID
{: #tenant-update-name-retrieve-tenant-id}

To get the tenant ID, see [Retrieving tenant information](/docs/logs-router?topic=logs-router-tenant-get).



## Choosing the management endpoint
{: #tenant-update-name-endpoint}


A tenant is the account-specific configuration of {{site.data.keyword.logs_routing_full_notm}} running within a region.

To get the details of a tenant in a region, you must use the management endpoint URL for the region where the tenant is configured.
{: important}

You can use private or public endpoints.

For more information, see [Management endpoint URLs](/docs/logs-router?topic=logs-router-endpoints).



## Changing the name by using the API
{: #tenant-update-name-change}

Run the following command to change the name of a tenant by using the **private endpoint**:

```sh
curl -X PATCH "https://management.private.${REGION}.logs-router.cloud.ibm.com/v1/tenants/${TENANT_ID}" \
--header 'Content-Type: application/json' \
--header "Authorization: ${IAM_TOKEN}" \
--header "IBM-API-Version: API_VERSION_DATE" \
--header 'If-Match: "ETAG"' \
--data '{"name":"NEW_TENANT_NAME"}'
```
{: pre}



Run the following command to change the name of a tenant by using the **public endpoint**:


```sh
curl -X PATCH "https://management.${REGION}.logs-router.cloud.ibm.com/v1/tenants/${TENANT_ID}" \
--header 'Content-Type: application/json' \
--header "Authorization: ${IAM_TOKEN}" \
--header "IBM-API-Version: API_VERSION_DATE" \
--header 'If-Match: "ETAG"' \
--data '{"name":"NEW_TENANT_NAME"}'
```
{: pre}


Where
- `REGION` defines the location where the tenant is configured.
- `IAM_TOKEN` defines the credentials that you use to authenticate your requests.
- `API_VERSION_DATE` defines the date of the API version that you want to use to query your tenant definition. The format must be as follows: `YYYY-MM-DD`
- `TARGET_DATA` defines the section where you enter the new tenant name.
- `TENANT_ID` defines the ID of the tenant that you are removing which was returned when the service was created.
- `ETAG` defines the ETag provided by GET, POST or PATCH.

For example, you can run the following cURL command to change the name:

% curl -X PATCH "https://management.us-south.logs-router.cloud.ibm.com/v1/tenants/fdd64011-7533-4539-8f45-67391ad295e5" \
--header 'Content-Type: application/json' \
--header "Authorization: ${IAM_TOKEN}" \
--header "IBM-API-Version: 2024-08-28" \
--header 'If-Match: "a351eeff801c1d2c3df9888ea16d23d10fee89c286c3c149f50e3076df3e360f"' \
--data '{"name":"my-new-tenant-name-in-dallas"}'
