---

copyright:
  years:  2023, 2024
lastupdated: "2024-08-27"

keywords:

subcollection: logs-router


---

{{site.data.keyword.attribute-definition-list}}


# About platform logs
{: #about-platform-logs}

Selected {{site.data.keyword.cloud}} services can generate logging data about their services. You can use the {{site.data.keyword.logs_routing_full_notm}} service to collect the data and route it to a destination of your choice. You can configure an {{site.data.keyword.logs_full_notm}} instance as a destination, or an {{site.data.keyword.la_full_notm}} instance.
{: shortdesc}

For example, the following diagram shows the high level view when the destination is an {{site.data.keyword.logs_full_notm}} instance:

![Flow of platform logs](/images/cloud-logs-platform-logs.png "Flow of platform logs"){: caption="Figure 1. Flow of platform logs" caption-side="bottom"}

Logs that are generated by {{site.data.keyword.cloud_notm}} services are called *platform logs*.
{: note}

## IAM permissions for managing platform logs in the account
{: #about-platform-logs-iam}

To manage the {{site.data.keyword.logs_routing_full_notm}} service in an account, you must have the `Manager` role for {{site.data.keyword.logs_routing_full_notm}}.

To see what IAM roles are available for {{site.data.keyword.logs_routing_full_notm}}, see [Managing IAM access](/docs/logs-router?topic=logs-router-iam).


## What platform logs are available
{: #about-platform-logs-collect}

Platform logs are generated by selected {{site.data.keyword.cloud_notm}} services in your account.

For information on the services generating platform logs, see [{{site.data.keyword.cloud_notm}} services that generate platform logs](/docs/logs-router?topic=logs-router-cloud_services).

To collect platform logs that are generated in a region where you operate in {{site.data.keyword.cloud_notm}}, you must configure the {{site.data.keyword.logs_routing_full_notm}} service in that region.
{: important}



## Where can you route platform logs
{: #about-platform-logs-route}

The following destinations are supported as destinations where the {{site.data.keyword.logs_routing_full_notm}} service can route platform logs:
- An {{site.data.keyword.logs_full_notm}} instance
- An {{site.data.keyword.la_full_notm}} instance


## Configuring the {{site.data.keyword.logs_routing_full_notm}} service
{: #about-platform-logs-config}

To configure the {{site.data.keyword.logs_routing_full_notm}} service, choose 1 of the following options:

- [Configuring the {{site.data.keyword.logs_routing_full_notm}} service to route platform logs to an {{site.data.keyword.logs_full_notm}} instance](/docs/logs-router?topic=logs-router-onboard-cloud-logs-tenant).

- [Configuring the {{site.data.keyword.logs_routing_full_notm}} service to route platform logs to an {{site.data.keyword.la_full_notm}} instance](/docs/logs-router?topic=logs-router-onboard-log-analysis-tenant).


## Fields that are included in platform logs
{: #about-platform-logs-2}

All platform logs are generated in JSON.

Platform logs include the following fields in addition to the log information:

`serviceName`
:   The [service name](/docs/account?topic=account-crn#service-name-crn) of the {{site.data.keyword.cloud_notm}} service generating the log.

`logSourceCRN`
:   The [CRN](/docs/account?topic=account-crn) of the {{site.data.keyword.cloud_notm}} service generating the log.

`saveServiceCopy`
:   A field indicating if the {{site.data.keyword.cloud_notm}} service generating the log receives a copy of the log back from {{site.data.keyword.logs_routing_full_notm}}

`tag`
:   A value for IBM only use.