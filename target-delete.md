---

copyright:
  years:  2023, 2024
lastupdated: "2024-09-12"

keywords:

subcollection: logs-router

---

{{site.data.keyword.attribute-definition-list}}

# Deleting a target from a tenant in {{site.data.keyword.logs_routing_full_notm}} by using the target ID
{: #target-delete}

You can delete a target from a tenant in {{site.data.keyword.logs_routing_full_notm}} that has 2 targets configured.
{: shortdesc}

{{site.data.content.tenant_definition_note}}


## Before you begin
{: #target-delete-prereqs}

Complete the following steps:

- Review [About {{site.data.keyword.logs_routing_full}}](/docs/logs-router?topic=logs-router-about) to understand concepts.

- Install all prerequisite tools as described in the [getting started](/docs/logs-router?topic=logs-router-getting-started&interface=ui#getting-started-before-you-begin).

- Set up permissions to manage targets in the account. For more information, see [Setting up IAM permissions for managing tenants](/docs/logs-router?topic=logs-router-iam&interface=ui).

- To get details on a tenant by using the API, check that you can connect to {{site.data.keyword.logs_routing_full_notm}} by using the management API. For more information, see [Connecting to {{site.data.keyword.logs_routing_full}}](/docs/logs-router?topic=logs-router-about#about_connecting).



## Retrieving the IAM bearer token
{: #target-delete-retrieve-iam-token}
{: api}

You must get an {{site.data.keyword.iamlong}} (IAM) access token to authenticate your requests to the {{site.data.keyword.logs_routing_full}} service. For more information, see [Retrieving an access token](/docs/logs-router?topic=logs-router-retrieve-access-token).

For example, you can retrieve your IAM bearer token and export it as an environment variable by running the following CLI command:

```sh
export IAM_TOKEN=`ibmcloud iam oauth-tokens --output json | jq -r '.iam_token'`
```
{: pre}

## Getting the tenant ID and target details
{: #target-delete-details}
{: api}

Deleting a target requires the tenant ID and information about the targets. If you do not remember your tenant ID, see [Retrieving tenant information in IBM Cloud Logs Routing](/docs/logs-router?topic=logs-router-tenant-get).

## Choosing the management endpoint
{: #target-delete-endpoint}
{: api}


A tenant is the account-specific configuration of {{site.data.keyword.logs_routing_full_notm}} running within a region.

To get the details of a tenant in a region, you must use the management endpoint URL for the region where the tenant is configured.
{: important}

You can use private or public endpoints.

For more information, see [Management endpoint URLs](/docs/logs-router?topic=logs-router-endpoints).


## Deleting a target
{: #target-delete-api}
{: api}

Run the following command to delete a target from a tenant by using the **private endpoint**:

```sh
curl -X DELETE  https://management.private.${REGION}.logs-router.cloud.ibm.com/v1/tenants/${TENANT_ID}/targets/<TARGET_ID> \
-H "IBM-API-Version: API_VERSION_DATE" \
-H "Authorization: ${IAM_TOKEN}"
```
{: pre}

Run the following command to delete a target from a tenant by using the **public endpoint**:

```sh
curl -X DELETE  https://management.${REGION}.logs-router.cloud.ibm.com/v1/tenants/${TENANT_ID}/targets/<TARGET_ID> \
-H "IBM-API-Version: API_VERSION_DATE" \
-H "Authorization: ${IAM_TOKEN}"
```
{: pre}


Where:

- `IAM_TOKEN` is the IAM token that you must use to authenticate the request.

- `TENANT_ID` defines the ID of the tenant that you are removing which was returned when the service was created.

- `REGION` defines the region where your tenant is located.

- `TARGET_ID` is the ID of the target that you want to delete.

- `API_VERSION_DATE` defines the date of the API version that you want to use to query your tenant definition. The format must be as follows: `YYYY-MM-DD`
