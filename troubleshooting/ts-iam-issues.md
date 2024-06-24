---

copyright:
  years: 2024
lastupdated: "2024-06-21"

keywords:

subcollection: logs-router

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Are you getting an IAM permission error when trying to use {{site.data.keyword.logs_routing_full_notm}}?
{: #ts-iam-permission-errors}
{: troubleshoot}
{: support}

Using {{site.data.keyword.logs_routing_full_notm}} requires various IAM permission for managing tenants and for ingestion.
{: shortdesc}

{{site.data.keyword.logs_routing_full_notm}} returns errors with a response code of `401` or `403` with the following text:
`Your access token is invalid or does not have the necessary permissions to perform this task`.
{: tsSymptoms}

Your IAM permissions are set up incorrectly.
{: tsCauses}

See [managing IAM access](/docs/logs-router?topic=logs-router-iam) and ensure you have the correct permissions
for the action you are trying to do.
{: tsResolve}
