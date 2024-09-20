---

copyright:
  years:  2023, 2024
lastupdated: "2024-09-20"

keywords:

subcollection: logs-router

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why can't I find logs routed to {{site.data.keyword.la_full_notm}}?
{: #ts-hostvalue}
{: troubleshoot}
{: support}

When searching for logs routed by {{site.data.keyword.logs_routing_full_notm}} to {{site.data.keyword.la_full_notm}}, the logs aren't found in the {{site.data.keyword.la_routing_full_notm}} UI.
{: shortdesc}

Users searching in the {{site.data.keyword.la_full_notm}} UI by the host value can not find routed logs.
{: tsSymptoms}

{{site.data.keyword.logs_routing_full_notm}} configures the host value to a value similar to `loc-xxxxxxx-0` instead of the service name.
{: tsCauses}

Change your {{site.data.keyword.la_full_notm}} query to use the service name (`serviceName`).
{: tsResolve}

For example, change a query similar to this:

```text
_platform:'Code Engine' label.Project:'ce-project-d6'
```
{: codeblock}

to:

```text
serviceName:codeengine label.Project:'ce-project-d6'
```
{: codeblock}

