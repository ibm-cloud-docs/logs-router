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


## Features
{: #about-v3-features}

By default, you can use {{site.data.keyword.logs_routing_full_notm}} that is based on the REST API V1 to route platform logs to a single destination per region.

With the release of {{site.data.keyword.logs_routing_full_notm}} REST API V3, you can migrate your account to use the V3 API so that you can route platform logs to multiple destinations and manage the service at the Enterprise account level by using enterprise IAM action controls. For more information on migration, see [Migrating to use {{site.data.keyword.logs_routing_full_notm}} REST API V3](/docs/logs-router?topic=logs-router-v3-migration){: external}.

The following table lists core features that the {{site.data.keyword.logs_routing_full_notm}} offers:

| Feature                                                                  | V1      |  V3      | More information |
|--------------------------------------------------------------------------|---------|----------|------------------|
| Consolidate platform logs to the region of your primary operations       | Yes     | Yes      | [Route platform logs to a single destination per region](/docs/logs-router?topic=logs-router-target-cloud-logs) |
| Improve your data residency compliance stature, keeping data at-rest within certain regions |  Yes     | Yes      | [About account settings](/docs/logs-router?topic=logs-router-settings-about)  |
| Route platform logs to multiple destinations adding flexibility at scale     | No      | Yes      | [Configuring multiple destinations](/docs/logs-router?topic=logs-router-route-manage) |
| Enterprise account management by using enterprise IAM action controls    | No      | Yes      | [Enterprise routing](/docs/logs-router?topic=logs-router-enterprise-routing-scenario&interface=terraform) |
{: caption="{{site.data.keyword.logs_routing_full_notm}} features" caption-side="top"}



## Advantages of migrating to {{site.data.keyword.logs_routing_full_notm}} V3
{: #about-v3-advantages}

- Add flexibility at scale with the multiple target routing capability.

    You can configure up to 16 different {{site.data.keyword.logs_full_notm}} target destinations. The location of the {{site.data.keyword.logs_full_notm}} instances can be in the same accoount or in different accounts.

- Send platform logs to multiple instances simultaneously, supporting both centralized compliance repositories and distributed operational logging ones.

    You can define intelligent routing rules that allow you to configure a central or distributed model for logging.

- Control data flow based on regional requirements, ensuring logs from specific locations reach appropriate destinations.

    You can configure with precision routing rules that define where to route data based on location.

- Design sophisticated segmentation strategies for your logs to enforce compliance and internal corporate requirements and regulations.

    You can define multiple targets and routing rules that allow you to send different types of logs to different destinations based on compliance and internal corporate requirements and regulations.

- Ensure no log is lost due to a misconfiguration or oversight.

    You can catch all logs generated in your account to ensure no logs are lost when they do not match specific routing rules.

- Support for Enterprise account management by using IAM Action Controls to protect your audit trail.

    Control at the enterprise account level how platform logs are routed across your Enterprise so account administrators that have full privileges within their accounts cannot modify or delete enterprise managed routes. You can meet compliance requirements while development teams maintain the flexibility they need for day to day operations.

## Concepts
{: #about-v3-concepts}

Before you can start configuring and using the {{site.data.keyword.logs_routing_full_notm}} service, you must configure the primary metadata location, and optionally the backup metadata location in the account settings. Next, you can define the destinations and rules that define how platform logs are routed in your the {{site.data.keyword.cloud_notm}} account.

The {{site.data.keyword.logs_routing_full_notm}} service uses two core concepts:
- `Target`

    A target defines where platform logs are collected.

    Targets represent your {{site.data.keyword.logs_full_notm}} destinations.

    Each target points to a specific {{site.data.keyword.logs_full_notm}} instance where data will be sent.

    For more information, see [Targets](/docs/logs-router?topic=logs-router-target_icl).

- `Route`

    A route defines the rules that indicate where platform logs that are generated in an account are routed.

    Routes define the logic for directing logs to targets.

    Routes evaluate the log source CRN (Cloud Resource Name) to determine the appropriate destination that you specify in a routing rule based on location.

    For more information, see [Routes](/docs/logs-router?topic=logs-router-route-manage).

You can configure the following type of target:

| Target                                      | Type                     | When to use |
|---------------------------------------------|--------------------------|------------|
| {{site.data.keyword.logs_full_notm}} | `cloud_logs`   | To view, search, and manage platform logs through the UI.
{: caption="List of targets" caption-side="top"}


## Enterprise account management
{: #about-v3-enterprise}

You can use {{site.data.keyword.logs_routing_full}} Enterprise routing to configure:
- Routing of platform logs from your enterprise child accounts to your enterprise parent account.

    You can configure your enterprise child accounts to route all or a subset of platform logs to an enterprise parent account. The child accounts contain Cloud resources that generate platform logs. The parent account owns the enterprise and has {{site.data.keyword.logs_full_notm}} instances that will contain the platform logs from the child accounts.

    The enterprise-managed routing configurations, from child accounts to parent account, can be locked by restricting the {{site.data.keyword.logs_routing_full}} enterprise actions with IAM Action Control. The child accounts can continue to manage their own account-managed {{site.data.keyword.logs_routing_full}} configurations, while the enterprise-managed configurations are restricted.

    ![A diagram that shows a sample {{site.data.keyword.logs_routing_full}} enterprise configuration.](/images/logs-router-enterprise-routing.svg "{{site.data.keyword.logs_routing_full}} enterprise configuration."){: caption="{{site.data.keyword.logs_routing_full}} enterprise configuration" caption-side="bottom"}

- Routing of platform logs from your enterprise child accounts to an {{site.data.keyword.logs_full_notm}} instance in another account of your choice.

You must use {{site.data.keyword.iamshort}} (IAM) Action Control to restrict changes to the enterprise-managed routing.
{: important}

Your child account administrators can separately define destinations to keep a copy of the platform logs in their account.
{: note}

You can configure Enterprise managed routing with IAM action controls to:
- Lock down compliance critical routes.
- Apply IAM restrictions that prevent child account administrators from modifying enterprise mandated routing configurations. Enterprise administrators control which targets and routes are immutable across all accounts.
- Maintain centralized governance while keeping operational autonomy in child accounts. Individual accounts can still create and manage their own additional routes for operational needs.
