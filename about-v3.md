---

copyright:
  years:  2023, 2026
lastupdated: "2026-04-28"

keywords:

subcollection: logs-router

---

{{site.data.keyword.attribute-definition-list}}




# About {{site.data.keyword.logs_routing_full_notm}} V3
{: #about-v3}

Use {{site.data.keyword.logs_routing_full_notm}} to configure how to route platform logs in your {{site.data.keyword.cloud_notm}} account.
{: shortdesc}


## Auditing events
{: #about-events}

platform logs are critical data for security operations and a key element for meeting compliance requirements.
{: note}

- Auditing events increase your visibility to {{site.data.keyword.cloud_notm}} configuration changes so you can manage the risk of incorrectly configured services more effectively.

- Auditing events simplify your understanding of IT complexity and agile development actions in the cloud. The combination of events provides a holistic view of what happened.

- Insights from the auditing event data help accelerate identification of abnormal activities. For example, you can track the frequency and volume of access management events or multi-factor authentication configuration changes.

The auditing data that is collected and routed complies with the Cloud Auditing Data Federation (CADF) standard.
{: note}

The CADF standard defines a full event model that includes the information that is needed to certify, manage, and auditing security of applications and services in cloud environments. The CADF event model includes the following components:
-	Action: The action is the operation or activity that an initiator performs, attempts to perform, or is waiting to complete.
-	Initiator: The initiator is the resource that makes an API call and generates a CADF event. The event that is triggered depends on the action that is requested by the API call.
-	Observer: The observer is the resource that creates and stores a CADF record from information available in a CADF event
-	Outcome: The outcome is the status of the action against the target.
-	Target: The target is the resource against which the action is performed, attempted to perform, or is pending to complete.



## Target locations
{: #about-locations}

Control of the storage location is critical to building enterprise-grade solutions on the {{site.data.keyword.cloud_notm}}.
{: note}

In {{site.data.keyword.atracker_full_notm}}, a target defines where platform logs are collected.  For more information, see [Targets](/docs/atracker?topic=atracker-atracker-resources#atracker-resources-targets).

You can configure different types of targets:

| Target                                      | Type                     | When to use |
|---------------------------------------------|--------------------------|------------|
| {{site.data.keyword.cos_full_notm}} (COS)   | `cloud_object_storage`   | To comply with Financial Services regulations. |
| {{site.data.keyword.logs_full_notm}}| `cloud_logs`   | To view, search, and manage auditing data through the UI. |
| {{site.data.keyword.messagehub_full}} | `event_streams`   | To send auditing data to data lakes, other analysis tools, and to other corporate tools such as Security Information and Event Management (SIEM) tools. |
{: caption="List of targets" caption-side="top"}

In {{site.data.keyword.atracker_full_notm}}, a route defines the rules that indicate where platform logs that are generated in an account are routed. For more information, see [Routes](/docs/atracker?topic=atracker-atracker-resources#atracker-resources-routes).


## Features
{: #about-features}

- Consolidate platform logs to the region of your primary operations

- Route platform logs to one or multiple locations

- Improve your data residency compliance stature, keeping data at-rest within certain regions

- Feed a data lake for analytics

- Forward your activity event data to {{site.data.keyword.cos_full_notm}} (COS) for {{site.data.keyword.cloud_notm}} for Financial Services compliance
