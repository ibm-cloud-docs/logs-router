---

copyright:
  years:  2023, 2025
lastupdated: "2025-03-21"

keywords:

subcollection: logs-router

---

{{site.data.keyword.attribute-definition-list}}

# Getting started with {{site.data.keyword.logs_routing_full}}
{: #getting-started}

Use the {{site.data.keyword.logs_routing_full_notm}} service to route platform logs from your {{site.data.keyword.cloud_notm}} account to your chosen target destination.
{: shortdesc}

![Flow of routed logs](/images/cloud-logs-platform-logs.png "Flow of routed logs"){: caption="Flow of routed logs" caption-side="bottom"}

As of 28 March 2024 the {{site.data.keyword.la_full_notm}} service is deprecated and will no longer be supported as of 30 March 2025.
{{site.data.keyword.logs_routing_full_notm}} will stop supporting `logdna` targets at the same time and no logs will be routed to these type of targets after that date.
You should make sure that you have configured {{site.data.keyword.logs_routing_full_notm}} to direct your logs to another destination before 30 March 2025.
Any `logdna` targets still configured after 30 April 2025 will be removed automatically from your {{site.data.keyword.logs_routing_full_notm}} configuration.
{: important}

Complete the following steps to start using {{site.data.keyword.logs_routing_full}}:

## Before you begin
{: #getting-started-before-you-begin}
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


## Retrieving the IAM bearer token
{: #getting-started-retrieve-iam-token}
{: step}

You must get an {{site.data.keyword.iamlong}} (IAM) access token to authenticate your requests to the {{site.data.keyword.logs_routing_full}} service. For more information, see [Retrieving an access token](/docs/logs-router?topic=logs-router-retrieve-access-token).

For example, you can retrieve your IAM bearer token and export it as an environment variable by running the following CLI command:

```sh
export IAM_TOKEN=`ibmcloud iam oauth-tokens --output json | jq -r '.iam_token'`
```
{: pre}

## Creating a service to service authorization
{: #getting-started-create-s2s}
{: step}

You must use {{site.data.keyword.iamlong}} (IAM) to create an authorization that grants {{site.data.keyword.logs_routing_full_notm}} access to {{site.data.keyword.logs_full_notm}} so the {{site.data.keyword.logs_routing_full_notm}} service can send logs to your {{site.data.keyword.logs_full_notm}} instance destination (target).

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

## Creating a tenant
{: #getting-started-create-tenant}
{: step}

When the {{site.data.keyword.logs_routing_full_notm}} console is first displayed, any existing target information is displayed.

If no target is configured for a region, the region displays the **Set target** option.

From the {{site.data.keyword.logs_routing_full_notm}} console, you can create a tenant and a target destination by configuring the option **Set target** for the region that you want to configure.


Complete the following steps:

1. [Log in to your {{site.data.keyword.cloud_notm}} account](https://cloud.ibm.com/login){: external}.

2. Click the **Menu** icon ![Menu icon](../icons/icon_hamburger.svg "Menu") &gt; **Observability**.

3. Click **Logging** > **Routing**.

4. Click **Set target**.

5. Click the tab **Cloud Logs** and select an {{site.data.keyword.logs_full_notm}} instance from the list. This is the instance where you want to receive logs that are routed by {{site.data.keyword.logs_routing_full_notm}}.

   You can select a {{site.data.keyword.logs_full_notm}} instance from the list.

   The {{site.data.keyword.logs_full_notm}} instance must be located in the same account that you are configuring.
   {: note}

6. Click **Save**.



## Verifying that logs are sent to the destination target
{: #getting-started-verify-logs}
{: step}

Verify that the logs for your cluster are routed to your {{site.data.keyword.logs_full_notm}} instance.

All platform logs are generated in JSON. You can filter platform logs in your instance be selecting the value of **ibm-platform-logs** as the `applicationName`.

Complete the following steps:

1. [Launch the {{site.data.keyword.logs_full_notm}} web UI](/docs/cloud-logs?topic=cloud-logs-instance-launch&interface=ui) for the {{site.data.keyword.logs_full_notm}} instance that is configured as the target to collect platform logs in a region. This is the instance that you selected in the step where you setup a target.
2. View logs through custom views. For more information, see [Viewing logs](/docs/cloud-logs?topic=cloud-logs-custom_views).
