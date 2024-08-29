---

copyright:
  years:  2023, 2024
lastupdated: "2024-08-29"

keywords:

subcollection: logs-router

---

{{site.data.keyword.attribute-definition-list}}

# Deleting a tenant from {{site.data.keyword.logs_routing_full_notm}}
{: #tenant-delete}

You can delete a tenant in {{site.data.keyword.logs_routing_full_notm}} through the UI, by using the API, or programmatically by using Terraform.
{: shortdesc}

{{site.data.content.tenant_definition_note}}


## Before you begin
{: #tenant-delete-prereqs}

Complete the following steps:

- Review [About {{site.data.keyword.logs_routing_full}}](/docs/logs-router?topic=logs-router-about) to understand concepts.

- Install all prerequisite tools as described in the [getting started](/docs/logs-router?topic=logs-router-getting-started&interface=ui#getting-started-before-you-begin-2).

- Set up permissions to manage targets in the account. For more information, see [Setting up IAM permissions for managing tenants](/docs/logs-router?topic=logs-router-tenant-iam-permissions).

- To get details on a tenant by using the API, check that you can connect to {{site.data.keyword.logs_routing_full_notm}} by using the management API. For more information, see [Connecting to {{site.data.keyword.logs_routing_full}}](/docs/logs-router?topic=logs-router-about#about_connecting).

- Deleting a tenant requires the tenant ID. If you do not remember your tenant ID, see [Retrieving tenant information in IBM Cloud Logs Routing](/docs/logs-router?topic=logs-router-tenant-get).


## Retrieving the IAM bearer token
{: #tenant-delete-retrieve-iam-token}
{: api}


You must get an {{site.data.keyword.iamlong}} (IAM) access token to authenticate your requests to the {{site.data.keyword.logs_routing_full}} service. For more information, see [Retrieving an access token](/docs/logs-router?topic=logs-router-retrieve-access-token).

For example, you can retrieve your IAM bearer token and export it as an environment variable by running the following CLI command:

```sh
export IAM_TOKEN=`ibmcloud iam oauth-tokens --output json | jq -r '.iam_token'`
```
{: pre}


## Retrieving the tenant ID
{: #tenant-delete-retrieve-tenantID}
{: api}

To get the tenant ID, see [Retrieving tenant information](/docs/logs-router?topic=logs-router-tenant-get).



## Choosing the management endpoint
{: #tenant-delete-endpoint}
{: api}

A tenant is the account-specific configuration of {{site.data.keyword.logs_routing_full_notm}} running within a region.

To get the details of a tenant in a region, you must use the management endpoint URL for the region where the tenant is configured.
{: important}

You can use private or public endpoints.

For more information, see [Management endpoint URLs](/docs/logs-router?topic=logs-router-endpoints).

## Deleting a tenant by using the API
{: #tenant-delete-api}
{: api}

The management endpoint that you call to remove a tenant is only available by using VPE.
{: important}

Run the following command to delete a tenant from the {{site.data.keyword.logs_routing_full_notm}} service by using the **private endpoint**:

```sh
curl -X DELETE  https://management.private.${REGION}.logs-router.cloud.ibm.com/v1/tenants/${TENANT_ID} \
-H "IBM-API-Version: API_VERSION_DATE" \
-H "Authorization: ${IAM_TOKEN}"
```
{: pre}

Run the following command to delete a tenant from the {{site.data.keyword.logs_routing_full_notm}} service by using the **public endpoint**:

```sh
curl -X DELETE  https://management.${REGION}.logs-router.cloud.ibm.com/v1/tenants/${TENANT_ID} \
-H "IBM-API-Version: API_VERSION_DATE" \
-H "Authorization: ${IAM_TOKEN}"
```
{: pre}

Where:

- `IAM_TOKEN` is the IAM token that you must use to authenticate the request.

- `TENANT_ID` defines the ID of the tenant that you are removing which was returned when the service was created.

- `REGION` defines the region where your tenant is located.

- `API_VERSION_DATE` defines the current date to request the latest version of the API. The valid format is `YYYY-MM-DD`. Any date up to the current date can be provided.





## Deleting a tenant and their targets through the UI
{: #tenant-delete-ui}
{: ui}

In the {{site.data.keyword.logs_routing_full_notm}} console, you can see if a target is configured for a region.

When you delete a target, you also delete the account tenant definition in the region. Therefore, if you have a tenant with 2 targets, and you delete a target in the UI, you will delete the 2 targets and the tenant.
{: important}

To delete a target for a {{site.data.keyword.logs_routing_full_notm}} tenant in a region, complete the following steps:

1. [Log in to your {{site.data.keyword.cloud_notm}} account](https://cloud.ibm.com/login){: external}.

2. Click the **Menu** icon ![Menu icon](../icons/icon_hamburger.svg "Menu") &gt; **Observability**.

3. Click **Logging** > **Routing**.

4. Click the ![Actions icon](../icons/action-menu-icon.svg "Actions") next to the region that you want to change.

5. Click **Delete target**.

6. Confirm to delete the target.
