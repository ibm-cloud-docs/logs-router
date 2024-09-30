---

copyright:
  years:  2023, 2024
lastupdated: "2024-09-30"

keywords:

subcollection: logs-router

---

{{site.data.keyword.attribute-definition-list}}

# Managing IAM access for {{site.data.keyword.logs_routing_full_notm}}
{: #iam}

Access to {{site.data.keyword.logs_routing_full}} service instances for users in your account is controlled by {{site.data.keyword.cloud}} Identity and Access Management (IAM). Every user that accesses the {{site.data.keyword.logs_routing_full_notm}} service in your account must be assigned an access policy with an IAM role. Review the following roles, actions, and more to help determine the best way to assign access to {{site.data.keyword.logs_routing_full_notm}}.
{: shortdesc}

The access policy that you assign users in your account determines what actions a user can do within the context of the service or specific instance that you select. The allowable actions are customized and defined by the {{site.data.keyword.logs_routing_full_notm}} as operations that are allowed to be done on the service. Each action is mapped to an IAM platform or service role that you can assign to a user. IAM access policies enable access to be granted at different levels.


When assigning roles, select the appropriate IAM role based on the actions you need to perform within {{site.data.keyword.logs_routing_full_notm}}. See [IAM actions by task](/docs/logs-router?topic=logs-router-iam&interface=cli#iam-bytask) for more information.

For information about how to assign roles by using the IBM Cloud CLI, see [Granting IAM permissions by using the CLI](/docs/logs-router?topic=logs-router-iam-permissions&interface=cli).

If you have the IAM permission to create policies and authorizations, you can grant only the level of access that you have as a user of the target service. For example, if you have viewer access for the target service, you can assign only the viewer role for the authorization. If you attempt to assign a higher permission such as administrator, it might appear that permission is granted, however, only the highest level permission you have for the target service, that is viewer, will be assigned. 
{: important}

If a specific role and its actions don't meet the needs of the use case that you're looking to address, you can [create a custom role](/docs/account?topic=account-custom-roles&interface=ui) and pick the actions to include.
{: tip}



## Managing access by using access groups
{: #groups}

An access group can be created to organize a set of users, service IDs, and trusted profiles into a single entity that makes it easy for you to assign access. You can assign a single policy to the group instead of assigning the same access multiple times for an individual user or service ID.

Access groups are assigned policies that grant roles and permissions to the members of that group. Members of an access group can include multiple identity types, like users, service IDs, and trusted profiles. The members inherit the policies, roles, and permissions that are assigned to the access group, and also keep the roles that they are assigned individually.

For more information, see [Setting up access groups](/docs/account?topic=account-groups).

To manage access or assign new access for users by using access groups, you must be the account owner, administrator, or editor on all Identity and Access enabled services in the account. Or, you can be the assigned administrator or editor for the IAM Access Groups Service.

Choose any of the following actions to manage access groups in the {{site.data.keyword.cloud_notm}}:

* [Creating an access group](/docs/account?topic=account-groups&interface=ui#create_ag).
* [Assigning access to a group](/docs/account?topic=account-groups&interface=ui#access_ag).

To get up and running quickly with IAM by setting up access groups for quick access assignments, inviting users to your account, and managing their access, see [Assigning access to resources by using access groups](/docs/account?topic=account-access-getstarted).
{: tip}



## Configuring Trusted Profiles
{: #configure-tp}

You can use trusted profiles to:
- Grant different {{site.data.keyword.cloud}} identities access to resources in your account
- Automatically grant federated users access to your account with conditions based on SAML attributes from your corporate directory.
- Set up fine-grained authorization for applications that are running in compute resources. This way, you aren't required to create service IDs or API keys for the compute resources.
- Establish trust with {{site.data.keyword.cloud}} services or service IDs in another account to grant cross-account access.

Choose any of the following actions to manage trusted profiles in the {{site.data.keyword.cloud_notm}}:

- To create a trusted profile, see [Creating trusted profiles](/docs/account?topic=account-create-trusted-profile).
- To update a trusted profile, see [Updating trusted profiles](/docs/account?topic=account-trusted-profile-update).
- To delete a trusted profile, see [Removing a trusted profile](/docs/account?topic=account-trusted-profile-remove).

When using Trusted Profiles for authentication, it's important that the `Compute Resource` is configured with the correct values. If necessary, follow the steps below to add or update the 'Compute Resource' in your Trusted Profile:

Adding or Updating Compute Resource:

1. Navigate to the IBM Cloud console and click on **Manage > Access (IAM)**.
2. Select **Trusted Profiles** from the menu.
3. Open your trusted profile and go to the `Compute Resource` section. A list where you can edit existing conditions or create new ones is displayed.
   - Choose the appropriate **Service Type**.
   - Add the condition `Namespace` to `ibm-observe`.

    These condition values match the default configuration for agent installation. If agent configuration values have been modified or additional conditions are required, ensure to configuration values accordingly.
    {: note}


## Managing access by assigning policies directly to users or service IDs
{: #users}

To assign user's access to resources you must be an administrator on all services in the account, or the assigned administrator for the particular service or service instance. To assign access to a service ID, you must be administrator on the identity service or the specific service ID.

Choose any of the following actions to manage access for users or service IDs by using IAM policies:

* To grant permissions, see [Assigning access](/docs/account?topic=account-assign-access-resources&interface=ui#assign-new-access).
* To revoke permissions, see [Removing access](/docs/account?topic=account-assign-access-resources&interface=ui#removing-access-console).
* To review assigned permissions, see [Reviewing your assigned access](/docs/account?topic=account-assign-access-resources&interface=ui#review-your-access-console).


### About service IDs
{: #serviceid}

A service ID identifies a service or application similar to how a user ID identifies a user. Service IDs are used to connect an application inside or outside of {{site.data.keyword.cloud}} to an {{site.data.keyword.cloud}} service. Service ID API keys inherit all access that is assigned to the specific service ID.

You can assign specific access policies to the service ID that restrict permissions for using specific services, or even combine permissions for accessing different services. Since service IDs are not tied to a specific user, if a user leaves an organization and is deleted from the account, the service ID remains. This way, your application or service stays up and running.

For more information, see [Creating and working with service IDs](/docs/account?topic=account-serviceids).

- [Creating a service ID](/docs/account?topic=account-serviceids&interface=ui#create_serviceid).
- [Updating a service ID](/docs/account?topic=account-serviceids&interface=ui#update_serviceid).
- [Deleting a service ID](/docs/account?topic=account-serviceids&interface=ui#delete_serviceid).
- To avoid a situation where your service ID is deleted causing an outage or disruption for the users of your service, you have the option to lock your service ID. Locking a service ID also prevents any policies from being changed, deleted, or assigned. For more information, see [Locking a service ID](/docs/account?topic=account-serviceids).


## IAM actions by task
{: #iam-bytask}

Review the available platform and service roles that are available, and the actions that are mapped to each to help you assign access.

| Action | IAM action | Administrator | Editor | Operator | Viewer |
|--------|------------|---------------|--------|-----------|--------|
| View console      | `logs-router.dashboard.view` | [Yes]{: tag-green} | [Yes]{: tag-green} | [Yes]{: tag-green} | [No]{: tag-red} |
{: caption="Table 1. IAM platform roles for {{site.data.keyword.logs_routing_full}}" caption-side="top"}


| Action | IAM action | Manager | Writer | Reader |
|--------|------------|---------|--------|--------|
| Configure platform logs   | `logs-router.tenant.create`  | [Yes]{: tag-green} | [No]{: tag-red} | [No]{: tag-red} |
| Delete the platform logs configuration                    | `logs-router.tenant.delete`  | [Yes]{: tag-green} | [No]{: tag-red} | [No]{: tag-red} |
| Edit (update) the platform logs configuration          | `logs-router.tenant.update`  | [Yes]{: tag-green} | [No]{: tag-red} | [No]{: tag-red} |
| View the platform logs configuration             | `logs-router.tenant.read`    | [No]{: tag-red} | [No]{: tag-red} | [Yes]{: tag-green} |
| Send log data to a destination                            | `logs-router.event.send`     | [No]{: tag-red} | [Yes]{: tag-green} | [No]{: tag-red} |
{: caption="Table 2. IAM service roles for {{site.data.keyword.logs_routing_full}}" caption-side="top"}




## {{site.data.keyword.cloud_notm}} platform roles
{: #iam-platform}

The following table detail actions that are mapped to platform roles.

Platform roles enable users to perform tasks on service resources at the platform level, for example, assign user access for the service, create or delete instances, and bind instances to applications.

Review the following tables that outline what types of tasks each role allows for when you're configuring {{site.data.keyword.logs_full_notm}} in your account.

Use the following table to identify the **Account management** **{{site.data.keyword.logs_routing_full_notm}}** platform roles that you can grant in the {{site.data.keyword.cloud_notm}} to run any of the following platform actions:

| Platform role            | Description of actions |
|--------------------------|------------------------|
| Viewer                   | As a viewer, you can view service instances, but you cannot modify them. |
| Operator                 | As an operator, you can perform platform actions required to configure and operate service instances, such as viewing a service's dashboard. |
| Editor                   | As an editor, you can perform all platform actions except for managing the account and assigning access policies. |
| Administrator            | As an administrator, you can perform all platform actions based on the resource where the role is being assigned, including assigning access policies to other users. |
| Service Configuration Reader | As a service configuration reader you can read the service configuration for governance management |
| Key Manager              | As a key manager you can manage resource keys, for example creating a new resource key for a resource instance |
{: caption="Table 1. IAM platform roles for {{site.data.keyword.logs_routing_full_notm}}" caption-side="top"}


## {{site.data.keyword.cloud_notm}} service roles
{: #iam-service}

The following table lists the service roles that are available in {{site.data.keyword.cloud_notm}}.

| Service role       | Description            |
|--------------------|------------------------|
| Manager            | As a manager, you have permissions beyond the writer role to complete privileged actions as defined by the service. In addition, you can create and edit service-specific resources.|
| Writer             | As a writer, you have permissions beyond the reader role, including creating and editing service-specific resources. |
| Reader             | As a reader, you can perform read-only actions within a service such as viewing service-specific resources. |
{: caption="Table 2. IAM service roles for {{site.data.keyword.logs_routing_full_notm}}" caption-side="top"}
