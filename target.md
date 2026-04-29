---

copyright:
  years:  2023, 2026
lastupdated: "2026-04-29"

keywords:

subcollection: logs-router

---

{{site.data.keyword.attribute-definition-list}}

# About targets
{: #target}

You can manage {{site.data.keyword.logs_full_notm}} targets in your account by using the {{site.data.keyword.logs_routing_full_notm}} UI, CLI, REST API V3, and Terraform scripts. A target is a resource where you can route platform logs that are generated in an {{site.data.keyword.cloud_notm}} account.
{: shortdesc}


## Understanding how targets work in your account
{: #target_behavior}

Note the following information about targets:

* You can define up to 16 targets in each account. Each account can have up to 2 default targets.

* A target defines a resource where platform logs are collected. [Routes](/docs/logs-router?topic=logs-router-route-manage) define how platform logs that are generated in the account are routed to the targets that you configure.

* You can define a target in any of the supported locations where {{site.data.keyword.logs_routing_full_notm}} is available. For more information, see [Locations](/docs/logs-router?topic=logs-router-locations).

* Targets are created within a region but are visible across regions. That is, all targets can be accessed by any {{site.data.keyword.logs_full_notm}} API endpoint.

* You can use private and public endpoints to manage targets. For more information about the list of `ENDPOINTS` that are available, see [Endpoints](/docs/logs-router?topic=logs-router-endpoints).

    * You can manage targets from the private network using an API endpoint with the following format: `https://api.private.REGION.logs-router.cloud.ibm.com`

    * You can manage targets from the public network using an API endpoint with the following format: `https://api.REGION.logs-router.cloud.ibm.com`

    * You can disable the public endpoints by updating the account settings. For more information, see [Enforcing private endpoints](/docs/logs-router?topic=logs-router-getting-started-mng-endpoints).


## Target types
{: #target_types}

You can configure any of the following target types:

| Target                                                             | Type                     | Learn more |
|--------------------------------------------------------------------|--------------------------|------------|
| {{site.data.keyword.logs_full_notm}}                          | `cloud_logs`   | [Managing {{site.data.keyword.logs_full_notm}} targets](/docs/logs-router?topic=logs-router-target_icl) |
{: caption="List of targets" caption-side="top"}



## IAM Access
{: #target_iam_access}

You must grant users IAM permissions to manage targets. For more information, see [Assign access to resources](/docs/account?topic=account-assign-access-resources).
{: note}

When you define a policy, you can indicate the scope of the permissions. You can choose from granting permissions for a specific region or for the entire account.

If you have the IAM permission to create policies and authorizations, you can grant only the level of access that you have as a user of the target service. For example, if you have viewer access for the target service, you can assign only the viewer role for the authorization. If you attempt to assign a higher permission such as administrator, it might appear that permission is granted, however, only the highest level permission you have for the target service, that is viewer, will be assigned.
{: important}

Users with regional scope will be limited to access targets in their authorized region.
{: important}

| IAM action               | IAM Policy scope  | IAM Roles                          | Description         |
| ------------------------ | ----------------- | ---------------------------------- | -------------- |
| `logs-router.target.read`   | Region            | `Administrator`  \n `Editor`  \n `Viewer`  \n `Operator` | Read (view) information about a target |
| `logs-router.target.create` | Region            | `Administrator`  \n `Editor` | Create a target |
| `logs-router.target.update` | Region            | `Administrator`  \n `Editor` | Update a target |
| `logs-router.target.delete` | Region            | `Administrator`  \n `Editor` | Delete a target |
| `logs-router.target.list`   | Account           | `Administrator`  \n `Editor`  \n `Viewer`  \n `Operator` | List all targets |
{: caption="IAM actions and the IAM roles that include them."}

## Authentication
{: #target_auth_opts}

When writing to a {{site.data.keyword.logs_full_notm}} target, you must configure a service-to-service (S2S) authorization between {{site.data.keyword.logs_routing_full_notm}} and {{site.data.keyword.logs_full_notm}}.
{: note}

Choose 1 of the following options:

- [Configuring S2S authorization using the UI](/docs/logs-router?topic=logs-router-iam-service-auth-logs-routing&interface=ui)

- [Configuring S2S authorization using the CLI](/docs/logs-router?topic=logs-router-iam-service-auth-logs-routing&interface=cli)

- [Configuring S2S authorization using the API](/docs/logs-router?topic=logs-router-iam-service-auth-logs-routing&interface=api)

- [Configuring S2S authorization using Terraform](/docs/logs-router?topic=logs-router-iam-service-auth-logs-routing&interface=terraform)





## Validating targets
{: #target_validate}

When you validate a target, you check that the credentials that are configured for a target are valid. These credentials are used by {{site.data.keyword.logs_routing_full_notm}} to authenticate with the destination target.

You can validate a target by using the {{site.data.keyword.metrics_router_full_notm}} CLI, the {{site.data.keyword.metrics_router_full_notm}} REST API, and Terraform scripts.

- [Validate via CLI](/docs/logs-router?topic=logs-router-target_icl&interface=cli#target_icl_get_cli)
- [Validate via API](/docs/logs-router?topic=logs-router-target_icl&interface=api#target_icl_api_view)


## CLI prerequisites
{: #target_cli}
{: cli}

Before you use the CLI to manage targets, complete the following steps:

1. [Install the {{site.data.keyword.cloud_notm}} CLI](/docs/cli?topic=cli-install-ibmcloud-cli).

2. [Install the {{site.data.keyword.logs_routing_full_notm}} CLI](/docs/logs-router?topic=logs-router-logs-router-cli-config).


## CLI commands
{: #target_v2_cli_cmd}
{: cli}

The following table lists the actions that you can run to manage targets:

| Action                     | Command                             |
|----------------------------|-------------------------------------|
| Create a target            | `ibmcloud logs-router target create`   |
| Update a target            | `ibmcloud logs-router target update`   |
| Delete a target            | `ibmcloud logs-router target delete`   |
| Read a target              | `ibmcloud logs-router target get`      |
| List all targets           | `ibmcloud logs-router target list`       |
{: caption="Target actions by using the {{site.data.keyword.logs_routing_full_notm}}Event Routing CLI" caption-side="top"}


For more information, see [{{site.data.keyword.logs_routing_full_notm}} CLI](/docs/logs-router?topic=logs-router-logs-router-cli).



## API targets and actions
{: #target_api}
{: api}

To make API calls to manage targets, complete the following steps:
1. Get an IAM access token. For more information, see [Retrieving IAM access tokens](/docs/logs-router?topic=logs-router-retrieve-iam-token).
2. Identify the API endpoint in the region where you plan to configure or manage a target. For more information, see [API Endpoints](/docs/logs-router?topic=logs-router-endpoints).

| Action                     | REST API Method  | API_URL                                          |
|----------------------------|------------------|--------------------------------------------------|
| Create a target            | `POST`           | `<ENDPOINT>/v3/targets`              |
| Update a target            | `PATCH`          | `<ENDPOINT>/v3/targets/<TARGET_ID>`  |
| Delete a target            | `DELETE`         | `<ENDPOINT>/v3/targets/<TARGET_ID>`  |
| Read a target              | `GET`            | `<ENDPOINT>/v3/targets/<TARGET_ID>`  |
| List all targets           | `GET`            | `<ENDPOINT>/v3/targets`              |
{: caption="Target actions by using the {{site.data.keyword.logs_routing_full_notm}} REST API" caption-side="top"}

You can use private and public endpoints to manage targets. For more information about the list of `ENDPOINTS` that are available, see [Endpoints](/docs/logs-router?topic=logs-router-endpoints).

- You can manage targets from the private network using an API endpoint with the following format: `https://api.private.REGION.logs-router.cloud.ibm.com`
- You can manage targets from the public network using an API endpoint with the following format: `https://api.REGION.logs-router.cloud.ibm.com`

- You can disable the public endpoints by updating the account settings. For more information, see [Configuring target and region settings](/docs/logs-router?topic=logs-router-settings).

For more information about the REST API, see [{{site.data.keyword.logs_routing_full_notm}} REST API v3](/apidocs/logs-router-service-api/logs-router-v3).
{: note}

## HTTP response codes
{: #target_rc}
{: api}

When you use the {{site.data.keyword.logs_routing_full_notm}} REST API, you can get standard HTTP response codes to indicate whether a method completed successfully.

- A 200 response always indicates success.
- A 4xx response indicates a failure.
- A 5xx response usually indicates an internal system error.

See the following table for some HTTP response codes:

| Status code | Status | Description |
|-------------|--------|-------------|
| `200` | OK | A list of targets were successfully retrieved. |
| `201` | OK | The request was successful. A resource is created. |
| `400` | Bad Request |	The request was unsuccessful. You might be missing a parameter that is required. |
| `401` | Unauthorized | The IAM token that is used in the API request is invalid or expired. |
| `403` | Forbidden | The operation is forbidden due to insufficient permissions. |
| `404` | Not Found |	The requested resource doesn't exist or is already deleted. |
| `409`|  Conflict | There is a conflict with the request data and the state of resources in system. |
| `429` | Too Many Requests |	Too many requests hit the API too quickly. |
| `500` | Internal Server Error |	Something went wrong. Your request could not be processed. Try again later. If the problem persists, note the transaction-id in the response header and contact [IBM Cloud support](https://watson.service-now.com/wcp).|
{: caption="List of HTTP response codes" caption-side="top"}



## Managing targets using the UI
{: #target-v2-ui}
{: ui}

You can manage your {{site.data.keyword.logs_routing_full_notm}} targets, routes, and settings using the IBM Console.

1. [Log in to your {{site.data.keyword.cloud_notm}} account](https://cloud.ibm.com/login){: external}.
2. Click the **Menu** icon ![Menu icon](../icons/icon_hamburger.svg) &gt; **Observability**.
3. Select **Logging > Routing**.


For more information, see [Managing {{site.data.keyword.logs_full_notm}} targets](/docs/logs-router?topic=logs-router-target_icl&interface=ui).
