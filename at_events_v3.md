---

copyright:
  years:  2023, 2026
lastupdated: "2026-05-06"

keywords:

subcollection: logs-router

---

{{site.data.keyword.attribute-definition-list}}



# Activity tracking events for {{site.data.keyword.logs_routing_full_notm}}
{: #at_events_v3}


{{site.data.keyword.cloud_notm}} services, such as {{site.data.keyword.logs_routing_full_notm}}, generate activity tracking events.
{: shortdesc}

Activity tracking events report on activities that change the state of a service in {{site.data.keyword.cloud_notm}}. You can use the events to investigate abnormal activity and critical actions and to comply with regulatory audit requirements.

You can use {{site.data.keyword.atracker_full_notm}}, a platform service, to route auditing events in your account to destinations of your choice by configuring targets and routes that define where activity tracking events are sent. For more information, see [About auditing events](/docs/atracker?topic=atracker-about).

You can use {{site.data.keyword.logs_full_notm}} to visualize and alert on events that are generated in your account and routed by {{site.data.keyword.atracker_full_notm}} to an {{site.data.keyword.logs_full_notm}} instance.


## Locations where activity tracking events are sent by {{site.data.keyword.logs_routing_full_notm}}
{: #at_events_v3-locations}


{{site.data.keyword.logs_routing_full_notm}} sends activity tracking events by {{site.data.keyword.logs_routing_full_notm}} in the regions that are indicated in the following table.




| Dallas (`us-south`) | Washington (`us-east`) | Toronto (`ca-tor`) | Montreal (`ca-mon`) | Sao Paulo (`br-sao`) |
|---------------------|------------------------|--------------------|---------------------|----------------------|
| [Yes]{: tag-green}  | [Yes]{: tag-green}     | [Yes]{: tag-green} | [Yes]{: tag-green}  | [Yes]{: tag-green}   |
{: caption="Regions where activity tracking events are sent in Americas locations" caption-side="top"}
{: #at_events_v3-table-1}
{: tab-title="Americas"}
{: tab-group="atracker"}
{: class="simple-tab-table"}
{: row-headers}


| Tokyo (`jp-tok`)   | Sydney (`au-syd`)  | Osaka (`jp-osa`)   | Chennai (`in-che`) | Mumbai (`in-mum`)  |
|--------------------|--------------------|--------------------|--------------------|--------------------|
| [Yes]{: tag-green} | [Yes]{: tag-green} | [Yes]{: tag-green} | [Yes]{: tag-green} | [Yes]{: tag-green} |
{: caption="Regions where activity tracking events are sent in Asia Pacific locations" caption-side="top"}
{: #at_events_v3-table-2}
{: tab-title="Asia Pacific"}
{: tab-group="atracker"}
{: class="simple-tab-table"}
{: row-headers}

| Frankfurt (`eu-de`) | London (`eu-gb`)   | Madrid (`eu-es`)   |
|---------------------|--------------------|--------------------|
| [Yes]{: tag-green}  | [Yes]{: tag-green} | [Yes]{: tag-green} |
{: caption="Regions where activity tracking events are sent in Europe locations" caption-side="top"}
{: #at_events_v3-table-3}
{: tab-title="Europe"}
{: tab-group="atracker"}
{: class="simple-tab-table"}
{: row-headers}


## Routing events using {{site.data.keyword.atracker_short}}
{: #at_events_ui_atracker}

{{site.data.keyword.atracker_short}} routes events based on the location that is specified in the `logSourceCRN` field included in the event.

You can define a target, the resource where events are routed to, in any {{site.data.keyword.atracker_short}} supported region. However, the target resource can be located in any region where that type of target is supported, in the same account or in a different account. For more information about supported targets, see [Targets](/docs/atracker?topic=atracker-atracker-resources#at_events_v3-resources-targets).

You can define rules to determine where auditing events are to be routed by configuring 1 or more routes in the account. You can define rules for managing global events and location-based events that are generated in regions where {{site.data.keyword.atracker_short}} is supported. For more information, see [supported regions](/docs/atracker?topic=atracker-regions).

To view events, you must access the target and download the object.

## Viewing activity tracking events for {{site.data.keyword.logs_routing_full_notm}}
{: #at-viewing}

You can use {{site.data.keyword.logs_full_notm}} to visualize and alert on events that are generated in your account and routed by {{site.data.keyword.atracker_full_notm}} to an {{site.data.keyword.logs_full_notm}} instance.

### Launching {{site.data.keyword.logs_full_notm}} from the Observability page
{: #log-launch-standalone}

For information on launching the {{site.data.keyword.logs_full_notm}} UI, see [Launching the UI in the {{site.data.keyword.logs_full_notm}} documentation.](/docs/cloud-logs?topic=cloud-logs-instance-launch)

## Management events
{: #at_events_mgt_atracker}

### Targets
{: #at_events_mgt_target}

The following table lists the auditing events that are generated when you manage targets:

| Action                                            | Description                |
|---------------------------------------------------|----------------------------|
| `logs-router.target.create`       | This event is generated when an administrator creates a new target.  |
| `logs-router.target.list` | This event is generated when an administrator lists all targets defined under a region. |
| `logs-router.target.read` | This event is generated when an administrator retrieves a target and its details by specifying the ID of the target.|
| `logs-router.target.update` | This event is generated when an administrator updates a target details by specifying the ID of the target. |
| `logs-router.target.delete` | This event is generated when an administrator deletes a target by specifying the ID of the target. |
{: caption="Events for managing targets" caption-side="top"}


### Routes
{: #at_events_mgt_route}

The following table lists the auditing events that are generated when you manage routes:

| Action                                            | Description                |
|---------------------------------------------------|----------------------------|
| `logs-router.route.create` | This event is generated when an administrator creates routing rules to direct log data to designated targets. |
| `logs-router.route.list` | This event is generated when an administrator lists routes.  |
| `logs-router.route.read` | This event is generated when an administrator retrieves a route and its details by specifying the ID of the route. |
| `logs-router.route.update` | This event is generated when an administrator replaces route details by specifying the ID of the route. You can also get this event when you validate a target by checking the credentials to the destination target. |
| `logs-router.route.delete` | This event is generated when an administrator deletes a route by specifying the ID of the route. |
{: caption="Events for managing routes" caption-side="top"}



### Settings
{: #at_events_setting}

The following table lists the auditing events that are generated when you manage settings:

| Action                                            | Description                |
|---------------------------------------------------|----------------------------|
| `logs-router.setting.update` | This event is generated when an administrator configures the {{site.data.keyword.logs_routing_full_notm}} settings for an account. |
| `logs-router.setting.get` | This event is generated when an administrator gets information about the {{site.data.keyword.logs_routing_full_notm}} settings for an account. |
{: caption="Events for managing settings" caption-side="top"}



### Destinations
{: #at_events_destinations}

The following table lists the auditing events that are generated when you work with destinations:

| Action                                            | Description                |
|---------------------------------------------------|----------------------------|
| `logs-router.destination.search` | This event is generated when an administrator queries the target destinations for a given set of cloud resources. |
{: caption="Events for managing settings" caption-side="top"}


### Migration to V3
{: #at_events_migration}

The following table lists the auditing events that are generated when you migrate from {{site.data.keyword.logs_routing_full_notm}} V1 to V3:

| Action                                            | Description                |
|---------------------------------------------------|----------------------------|
| `logs-router.migration.post` | This event is generated when an administrator initiates or completes the migration from {{site.data.keyword.logs_routing_full_notm}} version 1 to version 3. |
| `logs-router.migration.get` | This event is generated when an administrator retrieves the current migration state and version information. |
| `logs-router.migration.delete` | This event is generated when an administrator deletes all v3 routes and targets configurations and resets the migration state to BEFORE. |
{: caption="Events for managing settings" caption-side="top"}
