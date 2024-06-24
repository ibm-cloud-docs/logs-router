---

copyright:
  years:  2022, 2024
lastupdated: "2024-03-22"

keywords:

subcollection: logs-router

---

{{site.data.keyword.attribute-definition-list}}

# Deleting (offboarding) a tenant from {{site.data.keyword.logs_routing_full_notm}}
{: #offboarding-tenant}

You can delete (offboard) a tenant from {{site.data.keyword.logs_routing_full_notm}}.
{: shortdesc}

{{site.data.content.tenant_definition_note}}


## Before you begin
{: #offboarding-tenant-prereqs}

Be sure that you have completed the following steps:

1. Review [About {{site.data.keyword.logs_routing_full}}](/docs/logs-router?topic=logs-router-about) to understand concepts.

2. Review the [getting started](/docs/logs-router?topic=logs-router-getting-started) to understand configuration steps.

3. Install all prerequisite tools as described in the [getting started](/docs/logs-router?topic=logs-router-getting-started&interface=ui#getting-started-before-you-begin-2).

4. Set up permissions to manage targets in the account. For more information, see [Setting up IAM permissions for managing tenants](/docs/logs-router?topic=logs-router-tenant-iam-permissions).

5. To delete a target by using the API, check that you can connect to {{site.data.keyword.logs_routing_full_notm}} by using the management API. For more information, see [Enabling connectivity to manage tenants in {{site.data.keyword.logs_routing_full}}](/docs/logs-router?topic=logs-router-tenant-enable-connectivity).

### Getting the tenant ID
{: #offboard-tenant-get-id}

Deleting a tenant requires the tenant ID. If you do not remember your tenant ID, [you can retrieve it](/docs/logs-router?topic=logs-router-get-tenant&interface=api).
{: important}

## Retrieving the IAM bearer token
{: #offboarding-tenant-retrieve-iam-token-cli}
{: step}

You must get an {{site.data.keyword.iamlong}} (IAM) access token to authenticate your requests to the {{site.data.keyword.logs_routing_full}} service. For more information, see [Retrieving an access token](/docs/logs-router?topic=logs-router-retrieve-access-token).

For example, you can retrieve your IAM bearer token and export it as an environment variable by running the following CLI command:

```sh
export IAM_TOKEN=`ibmcloud iam oauth-tokens --output json | jq -r '.iam_token'`
```
{: pre}

## Deleting (offboarding) a tenant by using the API
{: #offboarding-tenant-api}
{: api}

The management endpoint that you call to remove a tenant is only available by using VPE.
{: important}

Run the following command to delete (offboard) a tenant from the {{site.data.keyword.logs_routing_full_notm}} service:

```sh
curl -X DELETE  https://management.private.${REGION}.logs-router.cloud.ibm.com/v1/tenants/${TENANT_ID} \
-H "IBM-API-Version: 2024-03-01" \
-H "Authorization: ${IAM_TOKEN}"
```
{: pre}

Where:

- `IAM_TOKEN` is the IAM token that you must use to authenticate the request.

- `TENANT_ID` defines the ID of the tenant that you are removing which was returned when the service was created (onboarded).

- `REGION` defines the region where your tenant is located.




## Deleting a target through the UI
{: #offboarding-tenant-ui}
{: ui}

When the {{site.data.keyword.logs_routing_full_notm}} console is first displayed, any existing target information is displayed.

To delete a target for a {{site.data.keyword.logs_routing_full_notm}} tenant in a region:

1. Click the ![Actions icon](../../icons/action-menu-icon.svg "Actions") next to the region that you want to change.

2. Click **Delete target**.

3. Confirm to delete the target.
