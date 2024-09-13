---

copyright:
  years:  2023, 2024
lastupdated: "2024-09-13"

keywords:

subcollection: logs-router

---

{{site.data.keyword.attribute-definition-list}}

# Configuring the {{site.data.keyword.logs_routing_full_notm}} service to route platform logs to an {{site.data.keyword.logs_full_notm}} instance in a region
{: #target-cloud-logs}
{: toc-content-type="tutorial"}
{: toc-services="logs-router, cloud-logs"}
{: toc-completion-time="30m"}

Use the {{site.data.keyword.logs_routing_full_notm}} service to route platform logs from your {{site.data.keyword.cloud_notm}} account to an {{site.data.keyword.logs_full_notm}} instance target destination. You must configure a tenant in the region and a target destination.
{: shortdesc}

{{site.data.content.tenant_definition_note}}

You must create a tenant in your account **in each region** where you want to use {{site.data.keyword.logs_routing_full_notm}}. Each region is independent and regions do not share data.
{: important}


## Goals
{: #target-cloud-logs-goals}

In this tutorial you will:
- Configure {{site.data.keyword.logs_routing_full_notm}} to route platform logs to an {{site.data.keyword.logs_full_notm}} instance in a region

- Verify that platform logs are being routed to your {{site.data.keyword.logs_full_notm}} instance.


## Before you begin
{: #target-cloud-logs-prereqs}

Complete the following steps:

1. Review [About {{site.data.keyword.logs_routing_full}}](/docs/logs-router?topic=logs-router-about) to understand concepts.

2. Install all prerequisite tools as described in the [getting started](/docs/logs-router?topic=logs-router-getting-started&interface=ui#getting-started-before-you-begin).

3. To get details on a tenant by using the API, check that you can connect to {{site.data.keyword.logs_routing_full_notm}} by using the management API. For more information, see [Connecting to {{site.data.keyword.logs_routing_full}}](/docs/logs-router?topic=logs-router-about#about_connecting).

4. Platform logs that are routed to {{site.data.keyword.logs_full_notm}} include metadata fields that you can use to manage the data and configure features in your {{site.data.keyword.logs_full_notm}} instance.

    The application name is the environment that produces and sends logs to {{site.data.keyword.logs_full_notm}}. Platform logs have the application name set to `ibm-platform-log`.

    The subsystem name is the service or application that produces and sends logs, or metrics to {{site.data.keyword.logs_full_notm}}. Platform logs have the subsystem name set to `CRNserviceName:instanceID`. For VPC platform logs, the fields is set to `is:resourceType`.

5. Set up permissions to manage targets in the account. For more information, see [Setting up IAM permissions for managing tenants](/docs/logs-router?topic=logs-router-tenant-iam-permissions).

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
{: #target-cloud-logs-s2s}
{: step}

You must use {{site.data.keyword.iamlong}} (IAM) to create an authorization that grants {{site.data.keyword.logs_routing_full_notm}} access to {{site.data.keyword.logs_full_notm}}.

### Creating a service to service authorization through the UI
{: #target-cloud-logs-s2s-ui}

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
{: #target-cloud-logs-s2s-cli}


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

    The output should start with `Bearer ` followed by a string containing letters, numbers, and symbols.

2. Create an authorization policy.

    ```text
    ibmcloud iam authorization-policy-create logs-router logs Sender
    ```
    {: codeblock}

For more information, see [Creating a S2S authorization to grant access to send logs to {{site.data.keyword.logs_full_notm}}](/docs/logs-router?topic=logs-router-iam-service-auth-logs-routing).

## Retrieving your {{site.data.keyword.logs_full_notm}} information
{: #target-cloud-logs-retrieve-information-cli}
{: step}

To route data to an {{site.data.keyword.logs_full_notm}} instance, you need information about the destination where you want logs delivered. You need the following information for your {{site.data.keyword.la_full_notm}} instance:
- The instance [CRN](/docs/account?topic=account-crn)
- The ingestion (ingress) endpoint and port

Only public ingress endpoints are supported when configuring a target to an {{site.data.keyword.logs_full_notm}} instance.
{: restriction}


Use this command to retrieve the URL and ID:

```text
ibmcloud resource service-instances --service-name logs --long --output JSON | jq '[.[] | {name: .name, id: .id, region: .region_id, ingestion_endpoint: .extensions.external_ingress}]'
```
{: pre}

If you have multiple instances, choose one you would like to use for receiving platform logs and save the information for later.

The endpoint is similar to:

```text
ingestion_endpoint: 3a622101-7521-4002-bf91-8c26e17eedcf.ingress.eu-de.logs.cloud.ibm.com
```
{: screen}

The ID is similar to:

```text
"crn:v1:bluemix:public:logs:eu-es:a/15ff25632a21af53a2aeed19a1542a44:3a622101-7521-4002-bf91-8c26e17eedcf::"
```
{: screen}


To get all the information about the instance, you can run `ibmcloud resource service-instances --service-name logs --long --output JSON`  This command lists all of your {{site.data.keyword.logs_full_notm}} instances. You need the following fields: `ID`that contains the instance's CRN, `extensions.external_ingress` that contains the ingress endpoint, and `extensions.virtual_private_endpoints.endpoints.ports.port_max` that contains the port value. {: tip}



## Creating a tenant and target
{: #target-cloud-logs-api-create}
{: step}

To receive your platform logs, you need to create a tenant with the {{site.data.keyword.logs_routing_full_notm}} service.

You must create your tenant in the region where you want to collect and route platform logs to the {{site.data.keyword.logs_full_notm}} instance.
{: important}

### Creating a tenant and target by using the API
{: #target-cloud-logs-api-create-api}

Run the following cURL request from the command line:

The create request creates the tenant in the region and the destination.{: note}

```sh
curl -X POST https://<MANAGEMENT-API-ENDPOINT>:443/v1/tenants \
-H "Content-Type: application/json" \
-H "Authorization: ${IAM_TOKEN}" \
-H 'IBM-API-Version: DATE' \
--data '{
    "name": "TENANT_NAME",
    "targets": [
        {
            "log_sink_crn": "CLOUD_LOGS_INSTANCE_ID",
            "name": "TARGET_NAME",
            "parameters": {
                "host": "CLOUD_LOGS_INSTANCE_INGRESS_ENDPOINT",
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
|`TARGET_NAME`| Name of the target destination. The name must be unique across all targets within an account-tenant and can be up to 35 characters long. The value can only contain these characters: `a-z,0-9,-./` You can for example choose the name of your {{site.data.keyword.logs_full_notm}} instance. An example would be `platformlogs-eu-es`. |
|`CLOUD_LOGS_INSTANCE_ID`| The CRN of your {{site.data.keyword.logs_full_notm}} instance. |
|`CLOUD_LOGS_INSTANCE_INGRESS_ENDPOINT`| The endpoint of your {{site.data.keyword.logs_full_notm}} instance. |
|`PORT` | Set to `443` |


Review your command carefully, since it contains values that were gathered previously. Be sure to make the necessary substitutions for the command to work as expected.
{: important}


If the creation request was successful, a response that contains your tenant metadata is returned. For example,

```json
{
    "id": "3517d2ed-9429-af34-ad52-34278391cbc8:",
    "created_at": "2023-10-20T18:30:00.143156Z",
    "updated_at": "2023-10-20T18:30:00.143156Z",
    "crn": "crn:v1:bluemix:public:logs-router:eu-de:a/3516b8fa0a174a71899f5affa4f18d78:3517d2ed-9429-af34-ad52-34278391cbc8::",
    "name": "tenant",
    "etag": "\"822b4b5423e225206c1d75666595714a11925cd0f82b229839864443d6c3c049\"",
    "targets": [
    {
        "id": "86432b-66a6-df12-7003-888a21a2b3",
        "log_sink_crn": "crn:v1:bluemix:public:logs:eu-gb:a/91d1d1e42f9b684df920bbd06461c222:743e3b4f-72bc-4669-9fa6-0e6621d0b232::",
        "name": "my-log-sink",
        "etag": "\"c3a43545a7f2675970671ac3a57b8db067a1866b2222e1b950ee8da612e347c6\"",
        "type": "logs",
        "created_at": "2023-10-20T18:30:00.143156Z",
        "updated_at": "2023-10-20T18:30:00.143156Z",
        "parameters": {
            "host": "743e3b4f-72bc-4669-9fa6-0e6621d0b232.ingress.eu-gb.logs.cloud.ibm.com",
            "port": 443
        }
    }
    ]
}
```
{: screen}

### Creating a tenant using the UI
{: #target-cloud-logs-api-create-ui}

Complete the following steps:

1. [Log in to your {{site.data.keyword.cloud_notm}} account](https://cloud.ibm.com/login){: external}.

2. Click the **Menu** icon ![Menu icon](../icons/icon_hamburger.svg "Menu") &gt; **Observability**.

3. Click **Logging** > **Routing**.

4. Click **Set target**.

5. Click the tab **Cloud Logs** and select an {{site.data.keyword.logs_full_notm}} instance from the list. This is the instance where you want to receive logs that are routed by {{site.data.keyword.logs_routing_full_notm}}.

   You can select a {{site.data.keyword.logs_full_notm}} instance from the list.

   The {{site.data.keyword.logs_full_notm}} instance must be located in the same account that you are configuring.{: important}

6. Click **Save**.


## Verifying that logs are sent to the destination target
{: #target-cloud-logs-verify}
{: step}

To ensure that your platform logs are successfully flowing to {{site.data.keyword.logs_full_notm}}, do the following steps:

1. [Launch the {{site.data.keyword.logs_full_notm}} web UI](/docs/cloud-logs?topic=cloud-logs-instance-launch) for the {{site.data.keyword.logs_full_notm}} instance that is configured as the target to collect platform logs in a region. This is the instance that you selected in the step where you setup a target.

2. View logs through custom views. For more information, see [Viewing logs](/docs/cloud-logs?topic=cloud-logs-custom_views).

All platform logs are generated in JSON. You can filter platform logs in your instance be selecting the value of **ibm-platform-logs** as the `applicationName`.
