---

copyright:
  years: 2024
lastupdated: "2024-06-21"

keywords:

subcollection: logs-router

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Are you getting connection failures when attempting to connect via a CSE proxy?
{: #ts-cse-connection-failure}
{: troubleshoot}
{: support}

The CSE proxy endpoints use a different port from the regular private and public endpoints.
Using the wrong port will result in network connection errors.
{: shortdesc}

Trying to use `curl` to reach the CSE proxy on the wrong port will result in error messages containing `SSL_ERROR_SYSCALL`.
Pointing the {{site.data.keyword.logs_routing_full_notm}} agent to the wrong port will yield similar error messages.
{: tsSymptoms}

The CSE proxy endpoints use port `3443` instead of the standard HTTPS port `443`.
{: tsCauses}

Use endpoints with the correct port, for example, `https://management.private.<REGION>.logs-router.cloud.ibm.com:3443/`
{: tsResolve}
