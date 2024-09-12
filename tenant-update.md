---

copyright:
  years:  2023, 2024
lastupdated: "2024-09-12"

keywords:

subcollection: logs-router

---

{{site.data.keyword.attribute-definition-list}}

# Modifying the target associated to a tenant through the {{site.data.keyword.logs_routing_full_notm}} UI
{: #tenant-update}

After you [create a tenant](/docs/logs-router?topic=logs-router-about&interface=ui#about_tenants) in your account, you can update information about your tenant through the UI.
{: shortdesc}

{{site.data.content.tenant_definition_note}}



## Before you begin
{: #tenant-update-prereqs}

Complete the following steps:

- Review [About {{site.data.keyword.logs_routing_full_notm}}](/docs/logs-router?topic=logs-router-about) to understand concepts.

- Install all prerequisite tools as described in the [getting started](/docs/logs-router?topic=logs-router-getting-started&interface=ui#getting-started-before-you-begin).

- Set up permissions to manage targets in the account. For more information, see [Setting up IAM permissions for managing tenants](/docs/logs-router?topic=logs-router-iam&interface=ui).

- To get details on a tenant by using the API, check that you can connect to {{site.data.keyword.logs_routing_full_notm}} by using the management API. For more information, see [Connecting to {{site.data.keyword.logs_routing_full}}](/docs/logs-router?topic=logs-router-about#about_connecting).




## Modifying the target associcated to a tenant through the {{site.data.keyword.logs_routing_full_notm}} UI
{: #tenant-update-ui}


Run the following command to modify a target configuration that is configured for a tenant:

1. [Log in to your {{site.data.keyword.cloud_notm}} account](https://cloud.ibm.com/login){: external}.

2. Click the **Menu** icon ![Menu icon](../icons/icon_hamburger.svg "Menu") &gt; **Observability**.

3. Click **Logging** > **Routing**.

4. Click the ![Actions icon](../icons/action-menu-icon.svg "Actions") next to the region that you want to change.

5. Click **Edit target**.

6. Choose the type of the target.

7. Select the new instance where you want to route platform logs.

    If the type is **Cloud Logs**, select the instance and click **Update**.

    If the type is **Log Analysis**, select the instance, the ingestion key, and then click **Update**.
