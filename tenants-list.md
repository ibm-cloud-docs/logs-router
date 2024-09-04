---

copyright:
  years:  2023, 2024
lastupdated: "2024-08-29"

keywords:

subcollection: logs-router

---

{{site.data.keyword.attribute-definition-list}}

# Listing tenants that are defined in the account
{: #tenants-list}

You can list the existing tenants that are configured in your {{site.data.keyword.cloud_notm}} account through the {{site.data.keyword.logs_routing_full_notm}} UI.
{: shortdesc}

{{site.data.content.tenant_definition_note}}

In the UI, you can only see 1 target configuration.
{: note}


## Prereqs
{: #tenants-list-prereqs}

- Review [About {{site.data.keyword.logs_routing_full}}](/docs/logs-router?topic=logs-router-about) to understand concepts.

- Set up permissions to manage targets in the account.

    To see information about the tenants that are configured in an account, you must use an ID that has the service role **reader** or the service role **manager**.

    For more information, see [Setting up IAM permissions for managing tenants](/docs/logs-router?topic=logs-router-tenant-iam-permissions).

- To get details of the targets that are configured for a tenant, you can use the API. For more information, see [Retrieving information on a tenant](/docs/logs-router?topic=logs-router-tenant-get).


## Listing tenants through the UI
{: #tenants-list-ui}

To list the tenants that are configured in the account, you must launch the {{site.data.keyword.logs_routing_full_notm}} console.

Complete the following steps to list the tenants in an account:

1. [Log in to your {{site.data.keyword.cloud_notm}} account](https://cloud.ibm.com/login){: external}.

	After you log in with your user ID and password, the {{site.data.keyword.cloud_notm}} dashboard opens.

2. Click the **Menu** icon ![Menu icon](../icons/icon_hamburger.svg "Menu") &gt; **Observability**.

3. Click **Logging** > **Routing**.

The {{site.data.keyword.logs_routing_full_notm}} console is displayed with your current configuration.
