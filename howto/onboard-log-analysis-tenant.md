---

copyright:
  years:  2022, 2024
lastupdated: "2024-03-22"

keywords:

subcollection: logs-router

---

{{site.data.keyword.attribute-definition-list}}

# Creating (onboarding) a tenant in {{site.data.keyword.logs_routing_full}}
{: #onboard-log-analysis-tenant}

You must create (onboard) a tenant in your account for the {{site.data.keyword.logs_routing_full}} service to manage logs in the {{site.data.keyword.cloud_notm}}.
{: shortdesc}

{{site.data.content.tenant_definition_note}}

You must create (onboard) a tenant in your account **in each region** where you want to use {{site.data.keyword.logs_routing_full_notm}}. Each region is independent and regions do not share data.
{: important}

## Before you begin
{: #onboard-log-analysis-tenant-before-you-begin}

Be sure that you have completed the following steps:

1. Review [About {{site.data.keyword.logs_routing_full}}](/docs/logs-router?topic=logs-router-about) to understand concepts.

2. Review the [getting started](/docs/logs-router?topic=logs-router-getting-started) to understand configuration steps.

3. Install all prerequisite tools as described in the [getting started](/docs/logs-router?topic=logs-router-getting-started&interface=ui#getting-started-before-you-begin-2).

4. Set up permissions to manage targets in the account. For more information, see [Setting up IAM permissions for managing tenants](/docs/logs-router?topic=logs-router-tenant-iam-permissions).

5. To create a target by using the API, check that you can connect to {{site.data.keyword.logs_routing_full_notm}} by using the management API. For more information, see [Enabling connectivity to manage tenants in {{site.data.keyword.logs_routing_full}}](/docs/logs-router?topic=logs-router-tenant-enable-connectivity).


## Retrieving the IAM bearer token
{: #onboard-log-analysis-tenant-retrieve-iam-token-cli}

You must get an {{site.data.keyword.iamlong}} (IAM) access token to authenticate your requests to the {{site.data.keyword.logs_routing_full}} service. For more information, see [Retrieving an access token](/docs/logs-router?topic=logs-router-retrieve-access-token).

For example, you can retrieve your IAM bearer token and export it as an environment variable by running the following CLI command:

```sh
export IAM_TOKEN=`ibmcloud iam oauth-tokens --output json | jq -r '.iam_token'`
```
{: pre}


## Retrieving your {{site.data.keyword.la_full_notm}} information
{: #onboard-log-analysis-tenant-retrieve-information}

To onboard as a {{site.data.keyword.la_full_notm}} tenant, you must supply information about the destination where you want logs delivered. You need to supply the following information for your {{site.data.keyword.la_full_notm}} instance:
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



## Creating a tenant by using the API
{: #onboard-log-analysis-tenant-api-create}
{: api}

Submit the create (onboard) request to {{site.data.keyword.logs_routing_full_notm}} by using the appropriate [management endpoint URL and port for the correct region](/docs/logs-router?topic=logs-router-endpoints).

```sh
curl -X POST https://<MANAGEMENT-API-ENDPOINT>:<PORT>/v1/tenants \
-H "Content-Type: application/json" \
-H "Authorization: ${IAM_TOKEN}" \
-H 'IBM-API-Version: CURRENT_DATE' \
--data "{
    "name": "TENANT_NAME",
    "targets": [
        {
            "log_sink_crn": "LOG_SINK_CRN",
            "name": "TARGET_NAME",
            "parameters": {
                "host": "LOG_SINK_INGESTION_ENDPOINT",
                "access_credential": "LOG_SINK_INGESTION_KEY",
                "port": LOG_SINK_PORT
            }
        }
    ]
}'
```
{: codeblock}

Where

- `<MANAGEMENT-API-ENDPOINT>` is the {{site.data.keyword.logs_routing_full}} endpoint in the region where you plan to collect logs. For more information, see [Endpoints](/docs/logs-router?topic=logs-router-endpoints). Make sure to use the corresponding port.
- `TENANT_NAME`: Name of the tenant. The name must be unique across all tenants for this account and can be up to 35 characters long.
- `TARGET_NAME`: Name of the target destination. The name must be unique across all targets for this tenant and can be up to 35 characters long.
- `LOG_SINK_INGESTION_KEY`: The ingestion key of the target {{site.data.keyword.la_full_notm}} instance.
- `LOG_SINK_CRN`: CRN of the target {{site.data.keyword.la_full_notm}} instance.
- `LOG_SINK_INGESTION_ENDPOINT`: Full qualified ingestion endpoint for the log-sink. You can choose a public or a private ingestion endpoint. For more information, see [{{site.data.keyword.la_full_notm}} ingestion endpoints](/docs/log-analysis?topic=log-analysis-endpoints#endpoints_ingestion_public).
- `LOG_SINK_PORT`: The corresponding port to the `LOG_SINK_INGESTION_ENDPOINT`.
- `DATE`: Specify the current date to request the latest version of the API. The valid format is `YYYY-MM-DD`. Any date up to the current date can be provided.

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
                "port": 8080,
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



## Creating a tenant through the UI
{: #onboard-log-analysis-tenant-ui-create}
{: ui}

When the {{site.data.keyword.logs_routing_full_notm}} console is first displayed, any existing target information is displayed.

If no target is configured for a region, the region displays the **Set target** option. When the target is set for the first time, an {{site.data.keyword.logs_routing_full_notm}} tenant is [created (onboarded)](/docs/logs-router?topic=logs-router-onboarding) and the target configured.

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
