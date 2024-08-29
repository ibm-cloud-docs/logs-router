---

copyright:
  years:  2023, 2024
lastupdated: "2024-08-29"

keywords:

subcollection: logs-router

---

{{site.data.keyword.attribute-definition-list}}

# Listing tenants that are defined in the account
{: #targets-list}

You can list existing tenants in {{site.data.keyword.logs_routing_full}} that are defined in the account.
{: shortdesc}

{{site.data.content.tenant_definition_note}}

An account can have only a single tenant within a region, but listing capability is provided for users who lost or forgot their tenant ID.
{: tip}


## Before you begin
{: #tenants-list-prereqs}

Complete the following steps:

1. Review [About {{site.data.keyword.logs_routing_full}}](/docs/logs-router?topic=logs-router-about) to understand concepts.

2. Install all prerequisite tools as described in the [getting started](/docs/logs-router?topic=logs-router-getting-started&interface=ui#getting-started-before-you-begin-2).

3. Set up permissions to manage targets in the account. For more information, see [Setting up IAM permissions for managing tenants](/docs/logs-router?topic=logs-router-tenant-iam-permissions).

4. To get details on a tenant by using the API, check that you can connect to {{site.data.keyword.logs_routing_full_notm}} by using the management API. For more information, see [Connecting to {{site.data.keyword.logs_routing_full}}](/docs/logs-router?topic=logs-router-about#about_connecting).



## Retrieving the IAM bearer token
{: #tenants-list-retrieve-iam-token-cli}

You must get an {{site.data.keyword.iamlong}} (IAM) access token to authenticate your requests to the {{site.data.keyword.logs_routing_full}} service. For more information, see [Retrieving an access token](/docs/logs-router?topic=logs-router-retrieve-access-token).

For example, you can retrieve your IAM bearer token and export it as an environment variable by running the following CLI command:

```sh
export IAM_TOKEN=`ibmcloud iam oauth-tokens --output json | jq -r '.iam_token'`
```
{: pre}


## Listing tenants by using the API
{: #tenants-list-api}
{: api}

Submit the list tenant request to {{site.data.keyword.logs_routing_full_notm}} by using the appropriate [management endpoint URL for the correct region](/docs/logs-router?topic=logs-router-endpoints).

The following example shows listing {{site.data.keyword.logs_routing_full_notm}} tenants in the `us-east` region by using a VPE:

```sh
curl -X GET https://management.private.us-east.logs-router.cloud.ibm.com:443/v1/tenants \
-H "Authorization: ${IAM_TOKEN}" \
-H "IBM-API-Version: 2024-03-01"
```
{: pre}

A successful request returns a response with an array that contains a single tenant, for example:

```json
{
  "tenants": [
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
          "log_sink_crn": "rn:v1:bluemix:public:logdna:us-east:a/473958g47b35f95747:48b580c-34ad-c985-1g2g-e1g75b71a2b3::",
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
  ]
}
```
{: screen}


## Listing tenants through the UI
{: #tenants-list-ui}
{: ui}


To list the tenants that are configured in the account, you must launch the {{site.data.keyword.logs_routing_full_notm}} console:

1. [Log in to your {{site.data.keyword.cloud_notm}} account](https://cloud.ibm.com/login){: external}.

	After you log in with your user ID and password, the {{site.data.keyword.cloud_notm}} dashboard opens.

2. Click the **Menu** icon ![Menu icon](../icons/icon_hamburger.svg "Menu") &gt; **Observability**.

3. Click **Logging** > **Routing**.

The {{site.data.keyword.logs_routing_full_notm}} console is displayed with your current configuration.
