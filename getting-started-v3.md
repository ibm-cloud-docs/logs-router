---

copyright:
  years:  2023, 2026
lastupdated: "2026-04-28"

keywords:

subcollection: logs-router

---

{{site.data.keyword.attribute-definition-list}}

# Getting started with {{site.data.keyword.logs_routing_full}} V3
{: #getting-started-v3}

Use the {{site.data.keyword.logs_routing_full_notm}} service to route platform logs from your {{site.data.keyword.cloud_notm}} account to your chosen target destination.
{: shortdesc}

![Flow of routed logs](/images/cloud-logs-platform-logs.png "Flow of routed logs"){: caption="Flow of routed logs" caption-side="bottom"}

Complete the following steps to start using {{site.data.keyword.logs_routing_full}}:

## Before you begin
{: #getting-started-v3-before-you-begin}
{: step}

1. If you don't have an {{site.data.keyword.cloud_notm}} account, [register an {{site.data.keyword.cloud_notm}} account](https://cloud.ibm.com/login){: external}. You need an IBMid to work in {{site.data.keyword.cloud_notm}}.

2. To configure platform logs, you must configure tenants and targets (destinations) in your {{site.data.keyword.cloud_notm}} account.

    {{site.data.content.tenant_definition-paragraph}}

    For more information, see [Learn more about {{site.data.keyword.logs_routing_full_notm}}](/docs/logs-router?topic=logs-router-about).

3. [Check the regions where the {{site.data.keyword.logs_routing_full_notm}} service is available](/docs/logs-router?topic=logs-router-locations). Identify a region where you operate in {{site.data.keyword.cloud_notm}} and check is in the list of supported regions.

4. Check that the user who is configuring {{site.data.keyword.logs_routing_full_notm}} for the {{site.data.keyword.cloud}} account has sufficient permissions to manage the {{site.data.keyword.logs_routing_full_notm}} service. For more information, see [Managing IAM access for {{site.data.keyword.logs_routing_full_notm}}](/docs/logs-router?topic=logs-router-iam).

    You need service role **reader** to view tenants and targets.

    You need service role **manager** to create, delete, update tenants and targets.

5. Install the following tools:

    - [Download and install the {{site.data.keyword.cloud}} CLI](/docs/cli).

    - Provision an {{site.data.keyword.logs_full_notm}} instance. For more information, see [Provisioning an instance](/docs/cloud-logs?topic=cloud-logs-instance-provision&interface=ui).

    - [Download and install jq](https://stedolan.github.io/jq/){: external} to process output and query desired results.


## Creating a service to service authorization
{: #getting-started-v3-create-s2s}
{: step}

You must use {{site.data.keyword.iamlong}} (IAM) to create an authorization that grants {{site.data.keyword.logs_routing_full_notm}} access to {{site.data.keyword.logs_full_notm}} so the {{site.data.keyword.logs_routing_full_notm}} service can send logs to your {{site.data.keyword.logs_full_notm}} instance destination (target).

You must have the **sender** role for {{site.data.keyword.logs_full_notm}} before you configure the service to service authorization.
{: important}

Complete the following steps:

1. In the {{site.data.keyword.cloud_notm}} console, click **Manage** > **Access (IAM)**, and select **Authorizations**.

2. Click **Create**.

3. Configure the source account. Select **This account**.

4. Select **Logs Routing** as the source service. Then, set the scope of the access to **All resources**.

5. Select **Cloud Logs** as the target service. Then, set the scope of the access to **All resources**, which grants access to all {{site.data.keyword.logs_full_notm}} instances, or a single instance by configuring **Resources based on selected attributes** &gt; **Service Instance**.

    Other attributes are not supported for this type of authorization.

6. In the *Service Access* section, select **Sender** to assign access to the source service that accesses the target service.

7. Click **Authorize**.


For more information, see [Creating a S2S authorization to grant access to send logs to {{site.data.keyword.logs_full_notm}}](/docs/logs-router?topic=logs-router-iam-service-auth-logs-routing).

## Configure the primary metadata location
{: #getting-started-v3-settings}
{: step}

Complete the following steps:

1. [Log in to your {{site.data.keyword.cloud_notm}} account](https://cloud.ibm.com/login){: external}.

2. Click the **Menu** icon ![Menu icon](../icons/icon_hamburger.svg) &gt; **Observability**.

3. Click **Logging >Routing**.

4. Click the **Settings** tab.

5. Click **Edit** next to the setting to be changed. Enter a **Primary Metadata location**.



## Create a target
{: #getting-started-v3-create-target}
{: step}

Complete the following steps to create a target:

1. [Log in to your {{site.data.keyword.cloud_notm}} account](https://cloud.ibm.com/login){: external}.
2. Click the **Menu** icon ![Menu icon](../icons/icon_hamburger.svg) &gt; **Observability**.
3. Select **Logging > Routing**.
4. In the *Targets* section, click **Create** to open the create panel.
5. In the *Choose destination*, pick **Search by instance** or **Specify CRN**.

    Select **Search by instance** to choose an {{site.data.keyword.logs_full_notm}} instance from the table that is available in the account

    Select **Specify CRN** to add the {{site.data.keyword.logs_full_notm}} instance CRN of an instance that is located in a different account.

6. In the *target details* section, complete the following tasks:

    Enter the target name.

    Enable the toggle from **Not set** to automatically set your new target as a default target in your {{site.data.keyword.logs_routing_full_notm}} settings.

7. Click **Create target**.


## Configure a route
{: #getting-started-v3-create-route}
{: step}

Complete the following steps to create a target:

1. [Log in to your {{site.data.keyword.cloud_notm}} account](https://cloud.ibm.com/login){: external}.
2. Click the **Menu** icon ![Menu icon](../icons/icon_hamburger.svg) &gt; **Observability**.
3. Select **Logging > Routing**.
4. In the *Routes* section, click **Create** to open the create panel.

## Verifying that logs are sent to the destination target
{: #getting-started-v3-verify-logs}
{: step}

Verify that the logs are routed to your {{site.data.keyword.logs_full_notm}} instance.

Complete the following steps:

1. [Launch the {{site.data.keyword.logs_full_notm}} web UI](/docs/cloud-logs?topic=cloud-logs-instance-launch&interface=ui) for the {{site.data.keyword.logs_full_notm}} instance that is configured as the target to collect platform logs in a region. This is the instance that you selected in the step where you setup a target.
2. View logs through custom views. For more information, see [Viewing logs](/docs/cloud-logs?topic=cloud-logs-custom_views).

    All platform logs are generated in JSON. You can filter platform logs in your instance be selecting the value of **ibm-platform-logs** as the `applicationName` or by running the Lucene query: `coralogix.metadata.applicationName:"ibm-platform-logs"`
