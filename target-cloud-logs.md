---

copyright:
  years:  2023, 2024
lastupdated: "2024-09-10"

keywords:

subcollection: logs-router

---

{{site.data.keyword.attribute-definition-list}}

# Configuring the {{site.data.keyword.logs_routing_full_notm}} service to route platform logs to an {{site.data.keyword.logs_full_notm}} instance
{: #target-cloud-logs}

You must create tenants in your account for the {{site.data.keyword.logs_routing_full}} service to manage logs that are generated in that region of the {{site.data.keyword.cloud_notm}}.
{: shortdesc}

{{site.data.content.tenant_definition_note}}

You must create a tenant in your account **in each region** where you want to use {{site.data.keyword.logs_routing_full_notm}}. Each region is independent and regions do not share data.
{: important}

## Before you begin
{: #target-cloud-logs-prereqs}

Complete the following steps:

1. Review [About {{site.data.keyword.logs_routing_full}}](/docs/logs-router?topic=logs-router-about) to understand concepts.

2. Install all prerequisite tools as described in the [getting started](/docs/logs-router?topic=logs-router-getting-started&interface=ui#getting-started-before-you-begin-2).

3. Set up permissions to manage targets in the account. For more information, see [Setting up IAM permissions for managing tenants](/docs/logs-router?topic=logs-router-tenant-iam-permissions).

4. To get details on a tenant by using the API, check that you can connect to {{site.data.keyword.logs_routing_full_notm}} by using the management API. For more information, see [Connecting to {{site.data.keyword.logs_routing_full}}](/docs/logs-router?topic=logs-router-about#about_connecting).

5. Platform logs that are routed to {{site.data.keyword.logs_full_notm}} include metadata fields that you can use to manage the data and configure features in your {{site.data.keyword.logs_full_notm}} instance.

    The application name is the environment that produces and sends logs to {{site.data.keyword.logs_full_notm}}. Platform logs have the application name set to `ibm-platform-log`.

    The subsystem name is the service or application that produces and sends logs, or metrics to {{site.data.keyword.logs_full_notm}}. Platform logs have the subsystem name set to `CRNserviceName:instanceID`. For VPC platform logs, the fields is set to `is:resourceType`.


## Retrieving the IAM bearer token
{: #target-cloud-logs-retrieve-iam-token-cli}
{: step}

You must get an {{site.data.keyword.iamlong}} (IAM) access token to authenticate your requests to the {{site.data.keyword.logs_routing_full}} service. For more information, see [Retrieving an access token](/docs/logs-router?topic=logs-router-retrieve-access-token).

For example, you can retrieve your IAM bearer token and export it as an environment variable by running the following CLI command:

```sh
export IAM_TOKEN=`ibmcloud iam oauth-tokens --output json | jq -r '.iam_token'`
```
{: pre}

## Creating a service to service authorization
{: #target-cloud-logs-s2s}
{: step}

You must define a service to service (S2S) authorization between {{site.data.keyword.logs_full_notm}} and {{site.data.keyword.logs_routing_full}} so the {{site.data.keyword.logs_routing_full}} service can send logs to your tenant.

Run the following command:

```text
ibmcloud iam authorization-policy-create logs-router logs Sender
```
{: codeblock}

For more information, see [Creating a S2S authorization to grant access to send logs to {{site.data.keyword.logs_full_notm}}](/docs/logs-router?topic=logs-router-iam-service-auth-logs-routing).

## Retrieving your {{site.data.keyword.logs_full_notm}} information
{: #target-cloud-logs-retrieve-information}
{: step}

You must supply information about the destination where you want logs delivered. You need to supply the following information for your {{site.data.keyword.la_full_notm}} instance:
- The instance [CRN](/docs/account?topic=account-crn)
- The ingress endpoint and port

Only public ingress endpoints are supported when configuring a target to an {{site.data.keyword.logs_full_notm}} instance.
{: restriction}

To obtain the instance CRN for the {{site.data.keyword.logs_full_notm}} instance, run this command:

```text
ibmcloud resource service-instances --service-name logs --long --output JSON
```
{: pre}

This command lists all of your {{site.data.keyword.logs_full_notm}} instances. You need the following fields:
- `ID` field: Contains the instance's CRN.
- `extensions.external_ingress`: Contains the ingress endpoint
- `extensions.virtual_private_endpoints.endpoints.ports.port_max`: Contains the port value.

The following sample shows some of the information that you can get:

```text
{
        "guid": "xxxxxxx",
        "id": "crn:v1:bluemix:public:logs:eu-gb:a/xxxxxxx:b951ceb1-06a4-4a1d-93d5-eaf47cedb86d::",
        "url": "/v2/resource_instances/b951ceb1-06a4-4a1d-93d5-eaf47cedb86d",
        "created_at": "2024-03-08T04:58:17.137066137Z",
        "updated_at": "2024-03-08T04:58:39.870280882Z",
        "deleted_at": null,
        "name": "My instance",
        "region_id": "eu-gb",
        "account_id": "xxxxxxx",
        "resource_plan_id": "0c1a62c7-a433-44df-a63e-b3fe8e629a94",
        "resource_group_id": "8f0e17c350d091415246eca5cd5239b1",
        "crn": "crn:v1:bluemix:public:logs:eu-gb:a/xxxxxxx:b951ceb1-06a4-4a1d-93d5-eaf47cedb86d::",
        "extensions": {
            "external_api": "xxxxxxx.api.eu-gb.logs.cloud.ibm.com",
            "external_dashboard": "https://dashboard.eu-gb.logs.appdomain.cloud/xxxxxxx/#/dashboard",
            "external_ingress": "xxxxxxx.ingress.eu-gb.logs.cloud.ibm.com",
            "virtual_private_endpoints": {
                "dns_domain": "private.eu-gb.logs.cloud.ibm.com",
                "dns_hosts": [
                    "xxxxxxx.ingress",
                    "xxxxxxx.api",
                    "dashboard"
                ],
                "endpoints": [
                    {
                        "ip_address": "10.12.93.39",
                        "zone": "eu-gb-1"
                    },
                    {
                        "ip_address": "10.12.94.38",
                        "zone": "eu-gb-2"
                    },
                    {
                        "ip_address": "10.12.95.38",
                        "zone": "eu-gb-3"
                    }
                ],
                "origin_type": "vpc",
                "ports": [
                    {
                        "port_max": 443,
                        "port_min": 443
                    }
                ]
            }
        },
        "create_time": 0,
        "created_by": "IBMid-xxxxxxx",
        "state": "active",
        "type": "service_instance",
        "resource_id": "cd515180-d78a-11ec-b396-db7d306c4f73",
        "dashboard_url": "https://cloud.ibm.com/observe/logging/xxxxxxx/overview",
        "allow_cleanup": false,
        "locked": false,
        "last_operation": {
            "type": "create",
            "state": "succeeded",
            "description": "service instance Provision succeeded",
            "updated_at": null,
            "cancelable": false
        },
        "account_url": "",
        "resource_plan_url": "",
        "resource_bindings_url": "/v2/resource_instances/xxxxxxx/resource_bindings",
        "resource_aliases_url": "/v2/resource_instances/xxxxxxx/resource_aliases",
        "siblings_url": "",
        "target_crn": "crn:v1:staging:public:globalcatalog::::deployment:0c1a62c7-a433-44df-a63e-b3fe8e629a94%3Aeu-xxx"
    }
```
{: screen}


## Creating a tenant and target by using the API
{: #target-cloud-logs-api-create}
{: step}


Submit the create request to {{site.data.keyword.logs_routing_full_notm}} by using the appropriate [management endpoint URL for the correct region](/docs/logs-router?topic=logs-router-endpoints).

The create requests creates the tenant in the region and the destination.{: important}

```sh
curl -X POST https://<MANAGEMENT-API-ENDPOINT>:443/v1/tenants \
-H "Content-Type: application/json" \
-H "Authorization: ${IAM_TOKEN}" \
-H 'IBM-API-Version: DATE' \
--data '{
    "name": "TENANT_NAME",
    "targets": [
        {
            "log_sink_crn": "CLOUD_LOGS_INSTANCE_CRN",
            "name": "TARGET_NAME",
            "parameters": {
                "host": "CLOUD_LOGS_INSTANCE_INGRESS_ENDPOINT",
                "port": CLOUD_LOGS_INSTANCE_TARGET_PORT
            }
        }
    ]
}'
```
{: codeblock}

Where

- `TENANT_NAME`: Name of the tenant. The name must be unique across tenants for this account and can be up to 35 characters long. The value can only contain these characters: `a-z,0-9,-./`
- `TARGET_NAME`: Name of the target destination. The name must be unique across all targets in the region and can be up to 35 characters long. The value can only contain these characters: `a-z,0-9,-./`
- `MANAGEMENT-API-ENDPOINT` is the {{site.data.keyword.logs_routing_full}} endpoint in the region where you plan to collect logs. For more information, see [Endpoints](/docs/logs-router?topic=logs-router-endpoints).
- `CLOUD_LOGS_INSTANCE_CRN` is the CRN of the {{site.data.keyword.logs_full_notm}} instance.
- `CLOUD_LOGS_INSTANCE_INGRESS_ENDPOINT` is the full qualified ingress endpoint for the destination of logs.
- `DATE`: Specify the current date to request the latest version of the API. The valid format is `YYYY-MM-DD`. Any date up to the current date can be provided.


The following example shows the creation of a tenant to {{site.data.keyword.logs_routing_full_notm}} in the `eu-es` region by using a VPE, and specifying target information for a {{site.data.keyword.logs_full_notm}} instance that is also in `eu-es`:

```sh
curl -X POST "https://management.private.eu-es.logs-router.cloud.ibm.com/v1/tenants" --header 'Authorization: Bearer TOKEN' --header 'Content-Type: application/json' --header 'IBM-API-Version: 2024-03-01' --data '{
    "name": "tenant",
    "targets": [
        {
            "log_sink_crn": "crn:v1:bluemix:public:logs:eu-es:a/91d1d1e42f9b684df920bbd06461c222:743e3b4f-72bc-4669-9fa6-0e6621d0b232::",
            "name": "my-log-sink",
            "parameters": {
                "host": "743e3b4f-72bc-4669-9fa6-0e6621d0b232.ingress.eu-es.logs.cloud.ibm.com",
                "port": 443
            }
        }
    ]
}'
```
{: pre}


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

## Setting application and subsystem name
{: #set-app-subsys}

If the `subsystemName` value is not specified, it is set to the `container name` if the `container name` is included in the metadata (for example, using the kubernetes filter).
The `applicationName` will be set to `namespace_name` if it is not set and included in the metadata.

It is possible to add static values for the application and subsystem name.
This can be done when installing the agent.
See [Managing the {{site.data.keyword.logs_routing_full}} agent for {{site.data.keyword.containerlong_notm}} clusters](/docs/logs-router?topic=logs-router-agent-std-cluster&interface=api#agent-std-cluster-deploy) or [Managing the {{site.data.keyword.logs_routing_full}} agent for {{site.data.keyword.openshiftlong_notm}} clusters](/docs/logs-router?topic=logs-router-agent-openshift&interface=api#agent-openshift-deploy) for more information.

In addition, a dynamic value for the application and subsystem name can be set depending on the log line.
The config map of the agent has to be modified to create the custom application name.
The same steps can be used to create a custom subsystem name.

For a JSON formatted message, do the following:

1. Add the `applicationName` to the log line, for example `{"level":"info", "msg":"test message", "applicationName":"my-application"}`

2. [Download the YAML configuration file for your environment.](/docs/logs-router?topic=logs-router-download-iclr-agent-configuration-file)

3. Open the downloaded YAML configuration file in a text editor.

4. Add a json parser to extract those fields:

   ```yaml
   [PARSER]
    Name json
    Format json
   ```
   {: codeblock}

3. Add the parser using a filter:

   ```yaml
   [FILTER]
      Name parser
      Match *
      Key_Name message
      Parser json
   ```
   {: codeblock}

4. Extract the application name using the nest filter:

   ```yaml
   [FILTER]
      Name nest
      Match *
      Operation lift
      Nested_under message
      Wildcard applicationName
   ```
   {: codeblock}

   `__applicationName__` must be at the top level since nested fields will not be extracted.
   {: note}


In this example, the `applicationName` for the log line would be `my-application`.

Add the following filters after the the configuration changes made in the previous steps. This filter will add default values for all log lines not containing an `applicationName` or `subsystemName`.

```yaml
  filter-add-ICL-meta-data.conf: |
    [FILTER]
        Name modify
        Match *
        Condition Key_Does_Not_Exist subsystemName
        Add subsystemName REPLACE_SUBSYSTEM_NAME

    [FILTER]
        Name modify
        Match *
        Condition Key_Does_Not_Exist applicationName
        Add applicationName REPLACE_APPLICATION_NAME
```
{: codeblock}
