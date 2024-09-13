---

copyright:
  years:  2023, 2024
lastupdated: "2024-09-13"

keywords:

subcollection: logs-router

---

{{site.data.keyword.attribute-definition-list}}

# Receiving platform logs in {{site.data.keyword.logs_full}}
{: #platformlogs2logs}
{: toc-content-type="tutorial"}
{: toc-services="logs-router, cloud-logs"}
{: toc-completion-time="30m"}

In this tutorial, you will set up the {{site.data.keyword.logs_routing_full_notm}} service to route platform logs from your {{site.data.keyword.cloud_notm}} account to your {{site.data.keyword.logs_full_notm}} instance.
{: shortdesc}

## Goals
{: #platformlogs2logs_goals}

In this tutorial you will:
- Configure {{site.data.keyword.logs_routing_full_notm}} to receive platform logs

- Verify that log data is flowing to your [{{site.data.keyword.logs_full_notm}}](/docs/cloud-logs) instance.

[//]: # "##########################################################################################"
[//]: # "Before you begin"
[//]: # "##########################################################################################"

## Before you begin
{: #platformlogs2logs_prereqs}
{: cli}

Before you begin, make sure you have the prerequisites.

 - Install the [{{site.data.keyword.cloud_notm}} CLI](/docs/cli?topic=cli-install-ibmcloud-cli).

 - Install the JSON CLI processor [(`jq`)](https://jqlang.github.io/jq/download/){: external}.

 - [Provision a {{site.data.keyword.logs_full_notm}} instance](/docs/cloud-logs?topic=cloud-logs-instance-provision) instance where you want to receive platform logs

 - In order to receive platform logs, you need a resource in your account emitting them.
You can provision any [IBM Cloud service that emits platform logs](/docs/logs-router?topic=logs-router-cloud_services) to test your setup.

## Before you begin
{: #platformlogs2logs_prereqs}
{: ui}

Before you begin, make sure you have the prerequisites.

 - [Provision a {{site.data.keyword.logs_full_notm}} instance](/docs/cloud-logs?topic=cloud-logs-instance-provision) instance where you want to receive platform logs

 - In order to receive platform logs, you need a resource in your account emitting them.
You can provision any [IBM Cloud service that emits platform logs](/docs/logs-router?topic=logs-router-cloud_services) to test your setup.


[//]: # "##########################################################################################"
[//]: # "Creating a service to service authorization"
[//]: # "##########################################################################################"

## Creating a service to service authorization using the CLI
{: #platformlogs2logs_s2s}
{: step}
{: cli}

You must use {{site.data.keyword.iamlong}} (IAM) to create an authorization that grants {{site.data.keyword.logs_routing_full_notm}} access to {{site.data.keyword.logs_full_notm}}.
The {{site.data.keyword.cloud_notm}} CLI is used to set up this policy.

1. Log in to your {{site.data.keyword.cloud_notm}} account. Include the `--sso` option if you are using a federated ID.

   ```text
   ibmcloud login
   ```
   {: pre}

2. Create the authorization policy.

    ```text
    ibmcloud iam authorization-policy-create logs-router logs Sender
    ```
    {: pre}

## Creating a service to service authorization using the UI
{: #platformlogs2logs_s2s}
{: step}
{: ui}

You must use {{site.data.keyword.iamlong}} (IAM) to create an authorization that grants {{site.data.keyword.logs_routing_full_notm}} access to {{site.data.keyword.logs_full_notm}}.
The {{site.data.keyword.cloud_notm}} CLI is used to set up this policy.

1. [Log in to your {{site.data.keyword.cloud_notm}} account](https://cloud.ibm.com/login){: external}.

2. In the {{site.data.keyword.cloud_notm}} console, click **Manage** > **Access (IAM)**, and select **Authorizations**.

3. Click **Create**.

4. Configure the source account. Select **This account**.

5. Select **Logs Routing** as the source service. Then, set the scope of the access to **All resources**.

6. Select **Cloud Logs** as the target service. Then, set the scope of the access to **All resources**, which grants access to all {{site.data.keyword.logs_full_notm}} instances, or a single instance by configuring **Resources based on selected attributes** &gt; **Service Instance**.

    Other attributes are not supported for this type of authorization.

7. In the *Service Access* section, select **Sender** to assign access to the source service that accesses the target service.

8. Click **Authorize**.

[//]: # "##########################################################################################"
[//]: # "Retrieving an IAM bearer token - not needed to create a tenant in the UI"
[//]: # "##########################################################################################"

## Retrieving an IAM bearer token using the CLI
{: #platformlogs2logs_IAM}
{: step}
{: cli}

Before you can set up the {{site.data.keyword.logs_routing_full_notm}} service to route your platform logs, you need an {{site.data.keyword.iamlong}} (IAM) access token for authentication. The {{site.data.keyword.cloud_notm}} CLI is used to obtain this information.

1. Log in to your {{site.data.keyword.cloud_notm}} account. Include the `--sso` option if you are using a federated ID.

   ```text
   ibmcloud login
   ```
   {: pre}

   The output will include your username similar to:
   ```text
   Targeted account Jane Doe\'s Account (15ff25632a21af53a2aeed19a1542a44) <-> 15ff256


    API endpoint:     https://cloud.ibm.com
    Region:           us-south
    User:             Jane.Doe@ibm.com
    Account:          Jane Doe\'s Account (15ff25632a21af53a2aeed19a1542a44) <-> 15ff256
    Resource group:   No resource group targeted, use 'ibmcloud target -g RESOURCE_GROUP'
    ```
    {: screen}

2. To grant access for using {{site.data.keyword.logs_routing_full_notm}} to your user, collect the username from the above generated output. In our example, this would be Jane.Doe@ibm.com. Replace `<username>` with your username in the following command:

    ```text
    ibmcloud iam user-policy-create <username> --roles Manager --service-name logs-router
    ```

    Instead of assigning roles directly to identities, a common strategy is to assign roles to access groups, and add identities as members to those access groups. For more information about access groups, see [setting up access groups.](/docs/account?topic=account-groups&interface=cli)
    {: tip}

3. To retrieve a token and export it as an environment variable, run the command

    ```text
    export IAM_TOKEN=`ibmcloud iam oauth-tokens --output json | jq -r '.iam_token'`
    ```
    {: pre}

4. Verify that your token has been correctly exported by displaying it in your terminal.

    ```text
    echo $IAM_TOKEN
    ```
    {: pre}

    The output should start with `Bearer ` followed by a string containing letters, numbers, and symbols.


[//]: # "##########################################################################################"
[//]: # "Determining your logging endpoint and ID -- not needed to create tenant in UI"
[//]: # "##########################################################################################"

## Determining your logging endpoint and ID using the CLI
{: platformlogs2logs_endpoint}
{: step}
{: cli}

The second piece of information that is needed is the ingestion endpoint and ID for your {{site.data.keyword.logs_full_notm}} instance. Use this command to retrieve the URL and ID:

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


[//]: # "##########################################################################################"
[//]: # "Creating a tenant"
[//]: # "##########################################################################################"

## Creating a tenant
{: platformlogs2logs_create}
{: step}
{: cli}

To receive your platform logs, you need to create a tenant with the {{site.data.keyword.logs_routing_full_notm}} service.

The {{site.data.keyword.logs_routing_full_notm}} service does currently not provide a CLI to manage tenants.
We will use the API to create the tenant instead.

You need to create your tenant in the region where you want to receive platform logs.
For more information, see [Endpoints](/docs/logs-router?topic=logs-router-endpoints).
Create your tenant by running a single command.

Review your command carefully, since it contains values that were gathered previously. Be sure to make the necessary substitutions for the command to work as expected.
{: important}

```sh
curl -X POST https://<MANAGEMENT-API-ENDPOINT>:443/v1/tenants \
-H "Content-Type: application/json" \
-H "Authorization: ${IAM_TOKEN}" \
-H 'IBM-API-Version: DATE' \
--data '{
    "name": "TENANT_NAME",
    "targets": [
        {
            "name": "TARGET_NAME",
            "log_sink_crn": "CLOUD_LOGS_INSTANCE_ID",
            "parameters": {
                "host": "CLOUD_LOGS_INSTANCE_INGRESS_ENDPOINT",
                "port": 443
            }
        }
    ]
}'
```
{: pre}

| Parameter  |  Description |
| ------------ | ------------ |
| `MANAGEMENT-API-ENDPOINT` | The endpoint of the {{site.data.keyword.logs_routing_full}} service. For example, `https://management.eu-es.logs-router.cloud.ibm.com` |
|`IAM_TOKEN`|The IAM Token we obtained previously. If you exported it in your environment as described above, it is replaced automatically. |
|`DATE`| The current date. For example, `2024-03-01`|
|`TENANT_NAME`| The name you want to give your tenant. An example would be `eu-es-tenant`. |
|`TARGET_NAME`|The name you want to give your target. You can for example choose the name of your {{site.data.keyword.logs_full_notm}} instance. An example would be `platformlogs-eu-es`. The name must be unique across all targets in the region and can be up to 35 characters long. The value can only contain these characters: `a-z,0-9,-./`|
|`CLOUD_LOGS_INSTANCE_ID`| The ID of your {{site.data.keyword.logs_full_notm}} instance we obtained previously. |
|`CLOUD_LOGS_INSTANCE_INGRESS_ENDPOINT`| The endpoint of your {{site.data.keyword.logs_full_notm}} instance we obtained previously. |


## Creating a tenant using the UI
{: platformlogs2logs_create}
{: step}
{: ui}

To receive your platform logs, you need to create a tenant with the {{site.data.keyword.logs_routing_full_notm}} service.
You need to create your tenant in the region where you want to receive platform logs.

1. [Log in to your {{site.data.keyword.cloud_notm}} account](https://cloud.ibm.com/login){: external}.

2. Click the **Menu** icon ![Menu icon](../icons/icon_hamburger.svg "Menu") &gt; **Observability**.

3. Click **Logging** > **Routing**.

4. Click **Set target**.

5. Click the tab **Cloud Logs** and select an {{site.data.keyword.logs_full_notm}} instance from the list. This is the instance where you want to receive logs that are routed by {{site.data.keyword.logs_routing_full_notm}}.

   You can select a {{site.data.keyword.logs_full_notm}} instance from the list.

   The {{site.data.keyword.logs_full_notm}} instance must be located in the same account that you are configuring.{: note}

6. Click **Save**.


[//]: # "##########################################################################################"
[//]: # "Verifying that logs are sent to the destination target"
[//]: # "##########################################################################################"

## Verifying that logs are sent to the destination target
{: #platformlogs2logs_verify}
{: step}

To ensure that your platform logs are successfully flowing to {{site.data.keyword.logs_full_notm}}, do the following steps:

1. [Launch the {{site.data.keyword.logs_full_notm}} web UI](/docs/cloud-logs?topic=cloud-logs-instance-launch) for the {{site.data.keyword.logs_full_notm}} instance that is configured as the target to collect platform logs in a region. This is the instance that you selected in the step where you setup a target.

2. View logs through custom views. For more information, see [Viewing logs](/docs/cloud-logs?topic=cloud-logs-custom_views).

All platform logs are generated in JSON. You can filter platform logs in your instance be selecting the value of **ibm-platform-logs** as the `applicationName`.
