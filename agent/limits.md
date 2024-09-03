---

copyright:
  years:  2023, 2024
lastupdated: "2024-09-03"

keywords:

subcollection: logs-router

---

{{site.data.keyword.attribute-definition-list}}

# Considerations and limitations when using {{site.data.keyword.logs_routing_full_notm}}
{: #limits}

There are processing considerations and limitations you need to be aware of when using {{site.data.keyword.logs_routing_full_notm}}.
{: shortdesc}

## Log length
{: #limit-log-length}

Logs processed by {{site.data.keyword.logs_routing_full_notm}} cannot exceed 16 K in length.

## Handling of stringify JSON
{: #stringify-json}

How stringify JSON is handled depends on how the log information is sent.

* If sent using the [{{site.data.keyword.logs_routing_full_notm}} agent](/docs/logs-router?topic=logs-router-agent-about), which uses Fluent Bit, any stringify JSON is unchanged.

* If sent using the [{{site.data.keyword.logs_routing_full_notm}} API](/apidocs/logs-router-service-api), stringify JSON has escape characters remove from text, log, and JSON fields as part of the API processing.
