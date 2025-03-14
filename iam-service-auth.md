---

copyright:
  years:  2023, 2024
lastupdated: "2024-11-12"

keywords:

subcollection: logs-router

---

{{site.data.keyword.attribute-definition-list}}

# Managing authorizations to grant access between services
{: #iam-service-auth}

Use {{site.data.keyword.iamlong}} (IAM) to create or remove an authorization that grants {{site.data.keyword.logs_routing_full_notm}} access to work with other services.
{: shortdesc}


## Authorizations
{: #iam-service-auth-1}

In a service to service (S2S) authorization:
- The source service is the service that is granted access to the target service.
- The roles that you select define the level of access for the source service.
- The target service is the service that you are granting permission to be accessed by the source service based on the roles that you assign.
- A source service can be in the same account where the authorization is created or in another account.
- The target service is always in the account where the authorization is created.

You can view whether the source service is located in the current account or another account by viewing the Source account column for the specific authorization on the [Authorizations](/iam/authorizations) page in the {{site.data.keyword.cloud}} console.
{: tip}

The following table lists the different S2S authorizations that you might need when you use the {{site.data.keyword.logs_routing_full_notm}} service:

| S2S Authorization | Source service | Target service |
|-------------------|----------------|----------------|
| Authorize sending logs to an {{site.data.keyword.logs_full_notm}} instance | {{site.data.keyword.logs_routing_full}} | {{site.data.keyword.logs_full_notm}} |
{: caption="S2S authorizations."}

For more information, see [Using authorizations to grant access between services](/docs/account?topic=account-serviceauth).



## Permissions to manage authorizations
{: #iam-service-auth-permissions}

You must have access to the target service to manage authorization between services.

The autorization that you define for the {{site.data.keyword.logs_routing_full_notm}} service requires that you have `Administrator` role for the target service.
{: important}

The following table outlines the permissions that are needed on the target service to be able to define an authorization:

| Action              | Administrator | Operator | Editor | Viewer |
|---------------------|---------------|----------|--------|--------|
| View all authorizations that are configured in the account | ![Checkmark icon](../icons/checkmark-icon.svg "checkmark") | | | |
| Create authorizations | ![Checkmark icon](../icons/checkmark-icon.svg "checkmark") | | | |
| Delete authorizations | ![Checkmark icon](../icons/checkmark-icon.svg "checkmark") | | | |
{: caption="Actions on the target service that are required to manage authorizations" caption-side="top"}

Users can only see authorizations that they configure in the account.

## Creating an authorization
{: #iam-service-auth-create}

Choose one of the following options to create a S2S authorization:
- [Authorize sending logs to an {{site.data.keyword.logs_full_notm}} instance](/docs/logs-router?topic=logs-router-iam-service-auth-logs-routing).


## Removing an authorization
{: #iam-service-mgmt-auth-remove-auth}

Choose one of the following options to remove a S2S authorization:
- [Remove a S2S authorization through the UI](/docs/logs-router?topic=logs-router-iam-service-auth-remove-auth&interface=ui)
- [Remove a S2S authorization by using the CLI](/docs/logs-router?topic=logs-router-iam-service-auth-remove-auth&interface=cli)
- [Remove a S2S authorization by using the API](/docs/logs-router?topic=logs-router-iam-service-auth-remove-auth&interface=api)
- [Remove a S2S authorization by using terraform](/docs/logs-router?topic=logs-router-iam-service-auth-remove-auth&interface=terraform)
