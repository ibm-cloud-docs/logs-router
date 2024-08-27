---

copyright:
  years:  2023, 2024
lastupdated: "2024-08-27"

keywords:

subcollection: logs-router

---

{{site.data.keyword.attribute-definition-list}}

# Deleting a tenant from {{site.data.keyword.logs_routing_full_notm}}
{: #tenant-delete}

You can delete a tenant from {{site.data.keyword.logs_routing_full_notm}}.
{: shortdesc}

{{site.data.content.tenant_definition_note}}


## Before you begin
{: #tenant-delete-prereqs}

Complete the following steps:

1. Review [About {{site.data.keyword.logs_routing_full}}](/docs/logs-router?topic=logs-router-about) to understand concepts.

2. Install all prerequisite tools as described in the [getting started](/docs/logs-router?topic=logs-router-getting-started&interface=ui#getting-started-before-you-begin-2).

3. Set up permissions to manage targets in the account. For more information, see [Setting up IAM permissions for managing tenants](/docs/logs-router?topic=logs-router-tenant-iam-permissions).

4. To get details on a tenant by using the API, check that you can connect to {{site.data.keyword.logs_routing_full_notm}} by using the management API. For more information, see [Connecting to {{site.data.keyword.logs_routing_full}}](/docs/logs-router?topic=logs-router-about#about_connecting).


### Getting the tenant ID
{: #tenant-delete-get-id}

Deleting a tenant requires the tenant ID. If you do not remember your tenant ID, see [Retrieving tenant information in IBM Cloud Logs Routing](/docs/logs-router?topic=logs-router-tenant-get).
{: important}

## Retrieving the IAM bearer token
{: #tenant-delete-retrieve-iam-token-cli}
{: step}

You must get an {{site.data.keyword.iamlong}} (IAM) access token to authenticate your requests to the {{site.data.keyword.logs_routing_full}} service. For more information, see [Retrieving an access token](/docs/logs-router?topic=logs-router-retrieve-access-token).

For example, you can retrieve your IAM bearer token and export it as an environment variable by running the following CLI command:

```sh
export IAM_TOKEN=`ibmcloud iam oauth-tokens --output json | jq -r '.iam_token'`
```
{: pre}

## Deleting a tenant by using the API
{: #tenant-delete-api}
{: api}

The management endpoint that you call to remove a tenant is only available by using VPE.
{: important}

Run the following command to delete a tenant from the {{site.data.keyword.logs_routing_full_notm}} service:

```sh
curl -X DELETE  https://management.private.${REGION}.logs-router.cloud.ibm.com/v1/tenants/${TENANT_ID} \
-H "IBM-API-Version: 2024-03-01" \
-H "Authorization: ${IAM_TOKEN}"
```
{: pre}

Where:

- `IAM_TOKEN` is the IAM token that you must use to authenticate the request.

- `TENANT_ID` defines the ID of the tenant that you are removing which was returned when the service was created.

- `REGION` defines the region where your tenant is located.




## Deleting a target through the UI
{: #tenant-delete-ui}
{: ui}

When the {{site.data.keyword.logs_routing_full_notm}} console is first displayed, any existing target information is displayed.

To delete a target for a {{site.data.keyword.logs_routing_full_notm}} tenant in a region, complete the following steps:

1. Click the ![Actions icon](../../icons/action-menu-icon.svg "Actions") next to the region that you want to change.

2. Click **Delete target**.

3. Confirm to delete the target.
