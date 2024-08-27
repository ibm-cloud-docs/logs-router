---

copyright:
  years:  2023, 2024
lastupdated: "2024-08-27"

keywords:

subcollection: logs-router

---

{{site.data.keyword.attribute-definition-list}}

# Managing targets by using the {{site.data.keyword.logs_routing_full_notm}} console
{: #lr_ui}

{{site.data.keyword.logs_routing_full}} provides an {{site.data.keyword.cloud_notm}} console that can be used to configure and review the {{site.data.keyword.logs_routing_full_notm}} routing configuration.
{: shortdesc}

{{site.data.keyword.logs_routing_full_notm}} uses the concept of tenants and targets.

{{site.data.content.tenant_definition-paragraph}}

## Viewing the configuration
{: #lr_ui_view}

To access the {{site.data.keyword.logs_routing_full_notm}} console:

1. [Log in to your {{site.data.keyword.cloud_notm}} account](https://cloud.ibm.com/login){: external}.

	After you log in with your user ID and password, the {{site.data.keyword.cloud_notm}} dashboard opens.

2. Click the **Menu** icon ![Menu icon](../../icons/icon_hamburger.svg "Menu") &gt; **Observability**.

3. Click **Logging** > **Routing**.

The {{site.data.keyword.logs_routing_full_notm}} console is displayed with your current configuration.

## Setting a target
{: #lr_ui_set}

When the {{site.data.keyword.logs_routing_full_notm}} console is first displayed, any existing target information is displayed.

If no target is configured for a region, the region displays the **Set target** option. When the target is set for the first time, an {{site.data.keyword.logs_routing_full_notm}} tenant is [created (onboarded)](/docs/logs-router?topic=logs-router-onboarding) and the target configured.

To set a target for a {{site.data.keyword.logs_routing_full_notm}} tenant in a region:

1. Click **Set target**.

2. Select an {{site.data.keyword.la_full_notm}} instance to receive logs routed by {{site.data.keyword.logs_routing_full_notm}}.

   You can select an {{site.data.keyword.la_full_notm}} instance by selecting an instance from the list and providing the instance [ingestion key](/docs/log-analysis?topic=log-analysis-ingestion_key) or by specifying an {{site.data.keyword.la_full_notm}} CRN and [ingestion key](/docs/log-analysis?topic=log-analysis-ingestion_key).

   Only {{site.data.keyword.la_full_notm}} instances in your account can be selected by name and ingestion key. If you want to route to an {{site.data.keyword.la_full_notm}} instance in another account, you must select the target by [CRN (Cloud Resource Name)](https://cloud.ibm.com/docs/account?topic=account-crn) and ingestion key. The CRN of an {{site.data.keyword.la_full_notm}} instance can be found by the account administrator of the {{site.data.keyword.la_full_notm}} instance by clicking ![Menu icon](../../icons/icon_hamburger.svg "Menu") > **Resource list** and clicking the {{site.data.keyword.la_full_notm}} instance. The CRN can be copied from the **Details** section.
   {: note}

3. Click **Save**.

## Editing a target
{: #lr_ui_edit}

When the {{site.data.keyword.logs_routing_full_notm}} console is first displayed, any existing target information is displayed.

To edit a target for a {{site.data.keyword.logs_routing_full_notm}} tenant in a region:

1. Click the ![Actions icon](../../icons/action-menu-icon.svg "Actions") next to the region that you want to change.

2. Click **Edit target**.

3. Select an {{site.data.keyword.la_full_notm}} instance to receive logs routed by {{site.data.keyword.logs_routing_full_notm}}.

   You can select an {{site.data.keyword.la_full_notm}} instance by selecting an instance from the list and providing the instance [ingestion key](/docs/log-analysis?topic=log-analysis-ingestion_key) or by specifying an {{site.data.keyword.la_full_notm}} CRN and [ingestion key](/docs/log-analysis?topic=log-analysis-ingestion_key).

   Only {{site.data.keyword.la_full_notm}} instances in your account can be selected by name and ingestion key. If you want to route to an {{site.data.keyword.la_full_notm}} instance in another account, you must select the target by [CRN (Cloud Resource Name)](https://cloud.ibm.com/docs/account?topic=account-crn) and ingestion key. The CRN of an {{site.data.keyword.la_full_notm}} instance can be found by the account administrator of the {{site.data.keyword.la_full_notm}} instance by clicking ![Menu icon](../../icons/icon_hamburger.svg "Menu") > **Resource list** and clicking the {{site.data.keyword.la_full_notm}} instance. The CRN can be copied from the **Details** section.
   {: note}
   {: note}

4. Click **Save**.

## Deleting a target
{: #lr_ui_delete}

When the {{site.data.keyword.logs_routing_full_notm}} console is first displayed, any existing target information is displayed.

To delete a target for a {{site.data.keyword.logs_routing_full_notm}} tenant in a region:

1. Click the ![Actions icon](../../icons/action-menu-icon.svg "Actions") next to the region that you want to change.

2. Click **Delete target**.

3. Confirm to delete the target.
