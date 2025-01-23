---

copyright:
  years:  2023, 2025
lastupdated: "2025-01-23"

keywords:

subcollection: logs-router

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Are you receiving a 304 status code in your {{site.data.keyword.logs_routing_full_notm}} agent?
{: #ts-target-disabled}
{: troubleshoot}
{: support}

{{site.data.keyword.logs_routing_full_notm}} disables your ingestion when the configured target is not reachable.
{: shortdesc}

{{site.data.keyword.logs_routing_full_notm}} returns errors with a response code of `304` which is logged in your agent with the following text: `"msg":"Failed to connect to ingester, retrying","error":"Connection Error websocket: bad handshake. Status Code: 304."`.
{: tsSymptoms}

Your target is set up incorrectly.
{: tsCauses}

To see the reason why your ingestion is disabled, follow the [instructions](/docs/logs-router?topic=logs-router-tenant-get-id) to get details about your tenant.
The details will include a `write_status` field in the response.
If the tenant is in the state `failed`, the response will contain the reason.
Use this information to enable your tenant.

For example, the following write status indicates that the service to service authorization between {{site.data.keyword.logs_full_notm}} and {{site.data.keyword.logs_routing_full}} is not set up correctly:

```json
{
    "write_status": {
        "status": "failed",
        "reason_for_last_failure": "Logs endpoint is not reachable. Received status code: 403",
        "last_failure": "2024-10-14T10:49:09Z"
    }
}
```
{: codeblock}

Follow the instructions to [create a service to service authorization](/docs/logs-router?topic=logs-router-iam-service-auth-logs-routing&interface=ui) to enable your tenant.
{: tsResolve}
