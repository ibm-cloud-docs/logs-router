---

copyright:
  years:  2023, 2025
lastupdated: "2025-04-30"

keywords:

subcollection: logs-router

---

{{site.data.keyword.attribute-definition-list}}

# Understanding your responsibilities when you are using {{site.data.keyword.logs_routing_full_notm}}
{: #shared-responsibilities}

Learn about the management responsibilities and terms and conditions that you have when you use {{site.data.keyword.logs_routing_full}}. For a high-level view of the service types in {{site.data.keyword.cloud}} and the breakdown of responsibilities between the customer and {{site.data.keyword.IBM_notm}} for each type, see [Shared responsibilities for {{site.data.keyword.cloud_notm}} offerings](/docs/overview?topic=overview-shared-responsibilities).
{: shortdesc}

Review the following sections for the specific responsibilities for you and for {{site.data.keyword.IBM_notm}} when you use {{site.data.keyword.logs_routing_full_notm}}. For the overall terms of use, see [{{site.data.keyword.cloud}} Terms and Notices](/docs/overview?topic=overview-terms).



## Incident and operations management
{: #incident-and-ops}

Incident and operations management includes tasks such as monitoring, event management, high availability, problem determination, recovery, and full state backup and recovery.

| Task  | {{site.data.keyword.IBM_notm}} Responsibilities | Your Responsibilities |
|----------|-----------------------|--------|
| Incident and operations management | Maintain the service and infrastructure workloads. | Maintain incident and operations management of your data. |
| Monitor incidents  | Provide notifications for planned maintenance, security bulletins, or unplanned outages. | Set preferences to [receive emails about platform notifications](/docs/account?topic=account-email-prefs).   \n  \n Monitor the [IBM Cloud status page](https://{DomainName}/status?selected=announcement) for general announcements. |
| Maintain {{site.data.keyword.cloud_notm}} high availability SLA for {{site.data.keyword.logs_routing_full_notm}}   | Provide {{site.data.keyword.logs_routing_full_notm}} functionality across availability zones in a Multi-Zone Region (MZR).    \n  \n Provide replication, fail-over features, and infrastructure maintenance and updates. |  |
{: row-headers}
{: caption="Responsibilities for incident and operations" caption-side="bottom"}
{: summary="The first column describes the task that the customer or IBM might be responsibility for. The second column describes {{site.data.keyword.IBM_notm}} responsibilities for that task. The third column describes your responsibilities as the customer for that task."}


## Change management
{: #change-management}


Change management includes tasks such as deployment, configuration, upgrades, patching, configuration changes, and deletion.

| Task | {{site.data.keyword.IBM_notm}} Responsibilities | Your Responsibilities |
|----------|-----------------------|--------|
| Updates to {{site.data.keyword.logs_routing_full_notm}} | Provide major, minor, and patch version updates for {{site.data.keyword.logs_routing_full_notm}} interfaces.   \n  \n Document changes in the [IBM Cloud Support Center](https://cloud.ibm.com/unifiedsupport/supportcenter) |  |
{: row-headers}
{: caption="Responsibilities for change management" caption-side="bottom"}
{: summary="The first column describes the task that the customer or IBM might be responsibility for. The second column describes {{site.data.keyword.IBM_notm}} responsibilities for that task. The third column describes your responsibilities as the customer for that task."}


## Identity and access management
{: #iam-responsibilities}

Identity and access management includes tasks such as authentication, authorization, access control policies, and approving, granting, and revoking access.

| Task | {{site.data.keyword.IBM_notm}} Responsibilities | Your Responsibilities |
|----------|-----------------------|--------|
| Manage permissions for {{site.data.keyword.logs_routing_full_notm}} | Lets you restrict access to the service.   \n  \n {{site.data.keyword.IBM_notm}} is responsible for the security and compliance of the {{site.data.keyword.logs_routing_full_notm}} service. | Restrict access to {{site.data.keyword.logs_routing_full_notm}} by using Cloud IAM access policies. Define IAM policies to control which users within your account have access to manage the service and related resources in your account.    \n  \n [Learn more about controlling access through IAM](/docs/logs-router?topic=logs-router-iam). |
{: row-headers}
{: caption="Responsibilities for identity and access management" caption-side="bottom"}
{: summary="The first column describes the task that a customer or IBM might be responsibility for. The second column describes {{site.data.keyword.IBM_notm}} responsibilities for that task. The third column describes your responsibilities as the customer for that task."}

## Security and regulation compliance
{: #security-compliance}

Security and regulation compliance includes tasks such as security controls implementation and compliance certification.

| Task | {{site.data.keyword.IBM_notm}} Responsibilities | Your Responsibilities |
|----------|-----------------------|--------|
| Meet security and compliance objectives  | Maintain controls that are commensurate with supported compliance standards, which might include standards such as SOC2, PCI, HIPAA, and Privacy Shield. \n  \n For more information see [Securing your data.](/docs/logs-router?topic=logs-router-mng-data) | Set up and maintain security and regulation compliance for your apps and data.   |
{: row-headers}
{: caption="Responsibilities for security and regulation compliance" caption-side="bottom"}
{: summary="The first column describes the task that the customer or IBM might be responsibility for. The second column describes {{site.data.keyword.IBM_notm}} responsibilities for that task. The third column describes your responsibilities as the customer for that task."}

## Disaster recovery
{: #disaster-recovery}


Disaster recovery includes tasks such as:

* Providing dependencies on disaster recovery sites
* Provisioning disaster recovery environments
* Backing up data and configuration information
* Replicating data and configuration information to the disaster recovery environment
* Failover on disaster events.

|  | {{site.data.keyword.IBM_notm}} Responsibilities | Your Responsibilities |
|----------|-----------------------|--------|
| Restore functionality for {{site.data.keyword.logs_routing_full_notm}}  | Automatically recover and restart {{site.data.keyword.logs_routing_full_notm}} components after any disaster event. | [Complete the disaster recovery (DR) steps for {{site.data.keyword.logs_routing_full_notm}}](/docs/logs-router?topic=logs-router-logs-router-ha-dr). |
| Backup {{site.data.keyword.logs_routing_full_notm}} components   | Daily backup of any data required to restore {{site.data.keyword.logs_routing_full_notm}} to full service. | `N/A` |
{: row-headers}
{: caption="Responsibilities for disaster recovery" caption-side="bottom"}
{: summary="The first column describes the task that the customer or IBM might be responsibility for. The second column describes {{site.data.keyword.IBM_notm}} responsibilities for that task. The third column describes your responsibilities as the customer for that task."}

Recovered and restarted service components will not reload customer log data.
{: note}
