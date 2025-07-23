---

copyright:
  years:  2023, 2025
lastupdated: "2025-07-23"


keywords: 

subcollection: logs-router

---

{{site.data.keyword.attribute-definition-list}}

# Activity tracking events for {{site.data.keyword.logs_routing_full_notm}}
{: #at_events}

{{site.data.keyword.cloud}} services, such as {{site.data.keyword.logs_routing_full_notm}}, generate activity tracking events.
{: shortdesc}

Activity tracking events report on activities that change the state of a service in {{site.data.keyword.cloud_notm}}. You can use the events to investigate abnormal activity and critical actions and to comply with regulatory audit requirements.

You can use {{site.data.keyword.atracker_full_notm}}, a platform service, to route auditing events in your account to destinations of your choice by configuring targets and routes that define where activity tracking events are sent. For more information, see [About {{site.data.keyword.atracker_full_notm}}](/docs/atracker?topic=atracker-about).

You can use {{site.data.keyword.logs_full_notm}} to visualize and alert on events that are generated in your account and routed by {{site.data.keyword.atracker_full_notm}} to an {{site.data.keyword.logs_full_notm}} instance.

## Locations where activity tracking events are generated
{: #at-locations}

{{site.data.keyword.logs_routing_full_notm}} sends activity tracking events by {{site.data.keyword.atracker_full_notm}} in the regions that are indicated in the following table.


| Dallas (`us-south`) | Washington (`us-east`)  | Toronto (`ca-tor`) | Sao Paulo (`br-sao`) |
|---------------------|-------------------------|-------------------|----------------------|
| [Yes]{: tag-green} | [Yes]{: tag-green} | [Yes]{: tag-green} | [Yes]{: tag-green} |
{: caption="Regions where activity tracking events are sent in Americas locations" caption-side="top"}
{: #atracker-table-1}
{: tab-title="Americas"}
{: tab-group="atracker"}
{: class="simple-tab-table"}
{: row-headers}




| Tokyo (`jp-tok`)    | Sydney (`au-syd`) |  Osaka (`jp-osa`) | Chennai (`in-che`) |
|---------------------|------------------|------------------|--------------------|
| [Yes]{: tag-green} | [Yes]{: tag-green} | [Yes]{: tag-green} | [No]{: tag-red} |
{: caption="Regions where activity tracking events are sent in Asia Pacific locations" caption-side="top"}
{: #atracker-table-2}
{: tab-title="Asia Pacific"}
{: tab-group="atracker"}
{: class="simple-tab-table"}
{: row-headers}

| Frankfurt (`eu-de`)  | London (`eu-gb`) | Madrid (`eu-es`) |
|---------------------------------------------------------------|---------------------|------------------|
| [Yes]{: tag-green} | [Yes]{: tag-green} | [Yes]{: tag-green} |
{: caption="Regions where activity tracking events are sent in Europe locations" caption-side="top"}
{: #atracker-table-3}
{: tab-title="Europe"}
{: tab-group="atracker"}
{: class="simple-tab-table"}
{: row-headers}


## Viewing activity tracking events for {{site.data.keyword.logs_routing_full_notm}}
{: #at-viewing}

You can use {{site.data.keyword.logs_full_notm}} to visualize and alert on events that are generated in your account and routed by {{site.data.keyword.atracker_full_notm}} to an {{site.data.keyword.logs_full_notm}} instance.

### Launching {{site.data.keyword.logs_full_notm}} from the Observability page
{: #log-launch-standalone}

For information on launching the {{site.data.keyword.logs_full_notm}} UI, see [Launching the UI in the {{site.data.keyword.logs_full_notm}} documentation.](/docs/cloud-logs?topic=cloud-logs-instance-launch)

## Management events
{: #at_actions}

| Action                      | Description                                                                                    |
|-----------------------------|------------------------------------------------------------------------------------------------|
| `logs-router.tenant.create` | This event is generated whenever a new tenant is created (onboarded).                                    |
| `logs-router.tenant.delete` | This event is generated whenever a tenant is deleted (offboarded).                                       |
| `logs-router.tenant.read`   | This event is generated whenever data about an existing tenant is viewed.                        |
| `logs-router.tenant.update` | This event is generated whenever the target data for a target of the tenant is edited (updated).        |
{: caption="Actions that generate management events" caption-side="bottom"}



## Analyzing events
{: #at_events_iam_analyze}

Depending on the action, the event includes additional information in the `requestData` or `responseData` field.
The following table lists custom fields that are included in these events:

| Custom fields                      | Valid values                               | Description                                             | Actions |
|------------------------------------|--------------------------------------------|---------------------------------------------------------|-----------|
| `requestData.region`               | For example, `eu-gb`                             | Defines the region where the tenant is located.       | create, read, update, delete, send |
| `requestData.targetType`           | For example, `logs`                            | Defines the target type requested.                      | create, update |
| `requestData.targetHost`           | For example, `logs.eu-gb.logging.cloud.ibm.com`  | Defines the host where logs are sent. | create, update |
| `requestData.targetPort`           | For example, `443`                               | Defines the port where logs are sent. | create, update |
| `requestData.targetCRN`     | A valid CRN                                      | Defines the CRN of the target.  | create, update |
| `requestData.tenantID`             | For example, `XXXXXXXX-XXXX-XXXX-XXXXXXXXXXXX`   | Defines the tenant ID. For example, the tenant ID to delete (offboard).                 | read, delete, update |
| `responseData.tenantCRN`           | For example, `crn:v1:staging:public:logs-router:eu-gb:a/XXXXXXXX-XXXX-XXXX-XXXXXXXXXXXX:XXXXXXXX-XXXX-XXXX-XXXXXXXXXXXX::` | Defines the CRN of the onboarded tenant. | create, read, update |
{: caption="Custom fields for events" caption-side="bottom"}
