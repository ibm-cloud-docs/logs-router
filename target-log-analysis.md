---
copyright:
  years:  2023, 2025
lastupdated: "2025-02-23"

keywords:

subcollection: logs-router

content-type: tutorial
services: logs-router, logdna
account-plan: paid
completion-time: 30m

---

{{site.data.keyword.attribute-definition-list}}

# Configuring the {{site.data.keyword.logs_routing_full_notm}} service to route platform logs to an {{site.data.keyword.la_full_notm}} instance in a region
{: #target-log-analysis}
{: toc-content-type="tutorial"}
{: toc-services="logs-router, logdna"}
{: toc-completion-time="30m"}

Use the {{site.data.keyword.logs_routing_full_notm}} service to route platform logs from your {{site.data.keyword.cloud_notm}} account to an {{site.data.keyword.la_full_notm}} instance target destination. You must configure a tenant in the region and a target destination.
{: shortdesc}

{{site.data.content.tenant_definition_note}}

You must create a tenant in your account **in each region** where you want to use {{site.data.keyword.logs_routing_full_notm}}. Each region is independent and regions do not share data.
{: important}


## Goals
{: #target-log-analysis-goals}

In this tutorial you will:
- Configure {{site.data.keyword.logs_routing_full_notm}} to route platform logs to an {{site.data.keyword.la_full_notm}} instance in a region

- Verify that platform logs are being routed to your {{site.data.keyword.la_full_notm}} instance.



## Before you begin
{: #target-log-analysis-prereqs}


Complete the following steps:

1. Review [About {{site.data.keyword.logs_routing_full}}](/docs/logs-router?topic=logs-router-about) to understand concepts.

2. Install all prerequisite tools as described in the [getting started](/docs/logs-router?topic=logs-router-getting-started&interface=ui#getting-started-before-you-begin).

3. To get details on a tenant by using the API, check that you can connect to {{site.data.keyword.logs_routing_full_notm}} by using the management API. For more information, see [Connecting to {{site.data.keyword.logs_routing_full}}](/docs/logs-router?topic=logs-router-about#about_connecting).

4. Platform logs that are routed to {{site.data.keyword.logs_full_notm}} include metadata fields that you can use to manage the data and configure features in your {{site.data.keyword.logs_full_notm}} instance.

    The application name is the environment that produces and sends logs to {{site.data.keyword.logs_full_notm}}. Platform logs have the application name set to `ibm-platform-log`.

    The subsystem name is the service or application that produces and sends logs, or metrics to {{site.data.keyword.logs_full_notm}}. Platform logs have the subsystem name set to `CRNserviceName:instanceID`. For VPC platform logs, the fields is set to `is:resourceType`.

5. Set up permissions to manage targets in the account. For more information, see [Granting IAM permissions](/docs/logs-router?topic=logs-router-iam-permissions&interface=ui).

    Before you can set up the {{site.data.keyword.logs_routing_full_notm}} service to route your platform logs, you need an {{site.data.keyword.iamlong}} (IAM) access token for authentication. The {{site.data.keyword.cloud_notm}} CLI is used to obtain this information.

    1. Log in to your {{site.data.keyword.cloud_notm}} account. Include the `--sso` option if you are using a federated ID.

       ```text
       ibmcloud login [--sso]
       ```
       {: pre}

       The output will include your username similar to:

       ```text
       Targeted account Jane Doe\'s Account (111111111112a21af53a2aeed19a1542a44) <-> 11111156


        API endpoint:     https://cloud.ibm.com
        Region:           us-south
        User:             Jane.Doe@ibm.com
        Account:          Jane Doe\'s Account (111111111112a21af53a2aeed19a1542a44) <-> 11111156
        Resource group:   No resource group targeted, use 'ibmcloud target -g RESOURCE_GROUP'
        ```
        {: screen}

    2. To grant access for using {{site.data.keyword.logs_routing_full_notm}} to your user, collect the username from the above generated output. In our example, this would be Jane.Doe@ibm.com. Replace `<username>` with your username in the following command:

        ```text
        ibmcloud iam user-policy-create <username> --roles Manager --service-name logs-router
        ```
        {: codeblock}

        Instead of assigning roles directly to identities, a common strategy is to assign roles to access groups, and add identities as members to those access groups. For more information about access groups, see [setting up access groups.](/docs/account?topic=account-groups&interface=cli)
        {: tip}



## Creating a service to service authorization
{: #target-log-analysis-s2s}
{: step}

You must use {{site.data.keyword.iamlong}} (IAM) to create an authorization that grants {{site.data.keyword.logs_routing_full_notm}} access to {{site.data.keyword.logs_full_notm}}.

### Creating a service to service authorization through the UI
{: #target-log-analysis-s2s-ui}

Complete the following steps:

1. [Log in to your {{site.data.keyword.cloud_notm}} account](https://cloud.ibm.com/login){: external}.

2. In the {{site.data.keyword.cloud_notm}} console, click **Manage** > **Access (IAM)**, and select **Authorizations**.

3. Click **Create**.

4. Configure the source account. Select **This account**.

5. Select **Logs Routing** as the source service. Then, set the scope of the access to **All resources**.

6. Select **Cloud Logs** as the target service. Then, set the scope of the access to **All resources**, which grants access to all {{site.data.keyword.logs_full_notm}} instances, or a single instance by configuring **Resources based on selected attributes** &gt; **Service Instance**.

    Other attributes are not supported for this type of authorization.

7. In the *Service Access* section, select **Sender** to assign access to the source service that accesses the target service.

8. Click **Authorize**.

### Creating a service to service authorization by using the CLI
{: #target-log-analysis-s2s-cli}


You must define a service to service (S2S) authorization between {{site.data.keyword.logs_full_notm}} and {{site.data.keyword.logs_routing_full}} so the {{site.data.keyword.logs_routing_full}} service can send logs to your tenant.

Complete the following steps:

1. Retrieving the IAM bearer token and export it as an environment variable. Run the following command:

    You must get an {{site.data.keyword.iamlong}} (IAM) access token to authenticate your requests to the {{site.data.keyword.logs_routing_full}} service. For more information, see [Retrieving an access token](/docs/logs-router?topic=logs-router-retrieve-access-token).

    For example, you can retrieve your IAM bearer token and export it as an environment variable by running the following CLI command:

    ```sh
    export IAM_TOKEN=`ibmcloud iam oauth-tokens --output json | jq -r '.iam_token'`
    ```
    {: pre}

    Verify that your token has been correctly exported by displaying it in your terminal.

    ```text
    echo $IAM_TOKEN
    ```
    {: pre}

    The output should start with `Bearer` followed by a string containing letters, numbers, and symbols.

2. Create an authorization policy.

    ```text
    ibmcloud iam authorization-policy-create logs-router logs Sender
    ```
    {: codeblock}

For more information, see [Creating a S2S authorization to grant access to send logs to {{site.data.keyword.logs_full_notm}}](/docs/logs-router?topic=logs-router-iam-service-auth-logs-routing).

## Retrieving your {{site.data.keyword.la_full_notm}} information
{: #target-log-analysis-info}
{: step}

To create a tenant with a target destination of tyle `logdna`, you must supply information about the destination where you want logs delivered. You need to supply the following information for your {{site.data.keyword.la_full_notm}} instance:
- The instance [CRN](/docs/account?topic=account-crn)
- The ingestion key
- The endpoint (hostname and port)

To obtain the instance CRN for the {{site.data.keyword.la_full_notm}} instance, run this command:

```text
ibmcloud resource service-instances --service-name logdna --long
```
{: pre}

This command lists all of your {{site.data.keyword.la_short}} instances. You can find the CRN for each in the `ID` field, for example:

```text
ID:                 crn:v1:bluemix:public:logdna:us-east:a/473958g47b35f95747:48b580c-34ad-c985-1g2g-e1g75b1a2b3c::
GUID:               48b580c-34ad-c985-1g2g-e1g75b1a2b3c
Name                Myinstance
Location            us-east
```
{: screen}

To retrieve the ingestion key for your instance, follow [these steps](/docs/log-analysis?topic=log-analysis-ingestion_key&interface=ui#ingestion_key_ui).

To determine the correct {{site.data.keyword.la_short}} endpoint information, including the hostname and port, see the list of [{{site.data.keyword.la_short}} endpoints](/docs/log-analysis?topic=log-analysis-endpoints).

## Creating a tenant
{: #target-log-analysis-tenant}
{: step}

To receive your platform logs, you need to create a tenant with the {{site.data.keyword.logs_routing_full_notm}} service.

You must create your tenant in the region where you want to collect and route platform logs to the {{site.data.keyword.la_short}} instance.
{: important}

### Creating a tenant by using the API
{: #target-log-analysis-tenant-api}

Run the following cURL request from the command line:

The create request creates the tenant in the region and the destination.{: note}

```sh
curl -X POST https://<MANAGEMENT-API-ENDPOINT>:<PORT>/v1/tenants \
-H "Content-Type: application/json" \
-H "Authorization: ${IAM_TOKEN}" \
-H 'IBM-API-Version: CURRENT_DATE' \
--data "{
    "name": "TENANT_NAME",
    "targets": [
        {
            "log_sink_crn": "LOG_ANALYSIS_INSTANCE_CRN",
            "name": "TARGET_NAME",
            "parameters": {
                "host": "LOG_ANALYSIS_INGESTION_ENDPOINT",
                "access_credential": "LOG_ANALYSIS_INSTANCE_INGESTION_KEY",
                "port": PORT
            }
        }
    ]
}'
```
{: codeblock}

Where

| Parameter  |  Description |
| ------------ | ------------ |
| `MANAGEMENT-API-ENDPOINT` | The endpoint of the {{site.data.keyword.logs_routing_full}} service. For more information, see [Endpoints](/docs/logs-router?topic=logs-router-endpoints). For example, `https://management.eu-es.logs-router.cloud.ibm.com` |
|`IAM_TOKEN`|The IAM Token you obtained previously. If you exported it in your environment as described above, it is replaced automatically. |
|`DATE`| The current date. For example, `2024-03-01`|
|`TENANT_NAME`| Name of the tenant. The name must be unique across tenants for this account and can be up to 35 characters long. The value can only contain these characters: `a-z,0-9,-./`  An example would be `eu-es-tenant`. |
|`TARGET_NAME`| Name of the target destination. The name must be unique across all targets within a region and can be up to 35 characters long. The value can only contain these characters: `a-z,0-9,-./` You can for example choose the name of your {{site.data.keyword.logs_full_notm}} instance. An example would be `platformlogs-eu-es`. |
|`LOG_ANALYSIS_INSTANCE_CRN`| The CRN of your {{site.data.keyword.la_full_notm}} instance. |
|`LOG_ANALYSIS_INGESTION_ENDPOINT`| The endpoint of your {{site.data.keyword.la_full_notm}} instance. You can choose a public or a private ingestion endpoint. For more information, see [{{site.data.keyword.la_full_notm}} ingestion endpoints](/docs/log-analysis?topic=log-analysis-endpoints#endpoints_ingestion_public).|
|`LOG_ANALYSIS_INSTANCE_INGESTION_KEY` | The ingestion key of the target {{site.data.keyword.la_full_notm}} instance. |
|`PORT` | Set to `443` |
{: caption="Parameter descriptions" caption-side="bottom"}


The following example shows the creation of a tenant to {{site.data.keyword.logs_routing_full_notm}} in the `us-east` region by using a VPE, and specifying target information for a {{site.data.keyword.la_full_notm}} instance that is also in `us-east`:

```sh
curl -X POST "https://management.private.us-east.logs-router.cloud.ibm.com/v1/tenants" \
--header "Authorization: Bearer TOKEN" \
--header 'Content-Type: application/json' \
--header 'IBM-API-Version: 2024-03-01' \
--data '{
    "name": "tenant",
    "targets": [
        {
            "log_sink_crn": "crn:v1:bluemix:public:logdna:us-east:a/3516b8fa0a174a71899f5affa4f18d78:3517d2ed-9429-af34-ad52-34278391cbc8::",
            "name": "my-log-sink",
            "parameters": {
                "host": "logs.us-east.logging.cloud.ibm.com",
                "port": 443,
                "access_credential": "an-ingestion-secret"
            }
        }
    ]
}'
```
{: pre}


If the creation (onboarding) request was successful, a response that contains your tenant metadata is returned. For example,

```json
{
    "id": "97543c-77b7-eg23-8114-999b31a2b3",
    "created_at": "2023-10-20T18:30:00.143156Z",
    "updated_at": "2023-10-20T18:30:00.143156Z",
    "crn": "crn:v1:bluemix:public:logs-router:eu-de:a/3516b8fa0a174a71899f5affa4f18d78:3517d2ed-9429-af34-ad52-34278391cbc8::",
    "name": "tenant",
    "etag": "\"822b4b5423e225206c1d75666595714a11925cd0f82b229839864443d6c3c049\"",
    "targets": [
    {
        "id": "86432b-66a6-df12-7003-888a21a2b3",
        "log_sink_crn": "crn:v1:bluemix:public:logdna:us-east:a/473958g47b35f95747:48b580c-34ad-c985-1g2g-e1g75b71a2b3::",
        "name": "my-log-sink",
        "etag": "\"c3a43545a7f2675970671ac3a57b8db067a1866b2222e1b950ee8da612e347c6\"",
        "type": "logdna",
        "created_at": "2023-10-20T18:30:00.143156Z",
        "updated_at": "2023-10-20T18:30:00.143156Z",
        "parameters": {
            "host": "logs.us-east.logging.cloud.ibm.com",
            "port": 443
        }
    }
    ]
}
```
{: screen}



### Creating a tenant through the UI
{: #target-log-analysis-ui}


When the {{site.data.keyword.logs_routing_full_notm}} console is first displayed, any existing target information is displayed.

If no target is configured for a region, the region displays the **Set target** option. When the target is set for the first time, an {{site.data.keyword.logs_routing_full_notm}} tenant is [created (onboarded)](/docs/logs-router?topic=logs-router-about&interface=ui#about_tenants) and the target configured.

To set a target for a {{site.data.keyword.logs_routing_full_notm}} tenant in a region, complete the following steps:

1. [Log in to your {{site.data.keyword.cloud_notm}} account](https://cloud.ibm.com/login){: external}.

    After you log in with your user ID and password, the {{site.data.keyword.cloud_notm}} dashboard opens.

2. Click the **Menu** icon ![Menu icon](../../icons/icon_hamburger.svg "Menu") &gt; **Observability**.

3. Click **Logging** > **Routing**.

    The {{site.data.keyword.logs_routing_full_notm}} console is displayed with your current configurations.

4. In the *Rules* section, configure a rule to create a tenant in a region. Choose the region where you want to create a tenant, then select **Set target**.

5. Click **Set target**. Then, configure a target destination by selecting an {{site.data.keyword.la_full_notm}} instance to receive logs routed by {{site.data.keyword.logs_routing_full_notm}}.

    You can choose to configure a target by searching for an instance in your {{site.data.keyword.cloud_notm}} account, or by entering the details of the instance.

    Option 1 **Search by instance**: You can select an {{site.data.keyword.la_full_notm}} instance by selecting an instance from the list and providing the instance [ingestion key](/docs/log-analysis?topic=log-analysis-ingestion_key) or by specifying an {{site.data.keyword.la_full_notm}} CRN and [ingestion key](/docs/log-analysis?topic=log-analysis-ingestion_key). You can only choose instances that are available in the same {{site.data.keyword.cloud_notm}} account where you are configuring the tenant.

    Option 2 **Specify CRN**: You can choose an instance that is available in the same {{site.data.keyword.cloud_notm}} account where you are configuring the tenant or an instance that is available in a different account. When you choose this option, you must enter the CRN of the target instance. If you want to route to an {{site.data.keyword.la_full_notm}} instance in another account, you must select the target by [CRN (Cloud Resource Name)](https://cloud.ibm.com/docs/account?topic=account-crn) and ingestion key. The CRN of an {{site.data.keyword.la_full_notm}} instance can be found by the account administrator of the {{site.data.keyword.la_full_notm}} instance by clicking ![Menu icon](../../icons/icon_hamburger.svg "Menu") > **Resource list** and clicking the {{site.data.keyword.la_full_notm}} instance. The CRN can be copied from the **Details** section.

3. Click **Save**.



## Verifying that logs are sent to the destination target
{: #target-log-analysis-verify}
{: step}

To ensure that your platform logs are successfully flowing to {{site.data.keyword.la_full_notm}} instance, do the following steps:

1. [Launch the web UI](/docs/log-analysis?topic=log-analysis-launch) for the {{site.data.keyword.la_full_notm}} instance that is configured as the target to collect platform logs in a region. This is the instance that you selected in the step where you setup a target.

2. View logs through custom views. For more information, see [Viewing logs](/docs/log-analysis?topic=log-analysis-views).

All platform logs are generated in JSON. You can filter platform logs in your instance be selecting the value of **ibm-platform-logs** as the `applicationName`.
