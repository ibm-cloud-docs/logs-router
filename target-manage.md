---

copyright:
  years:  2023, 2026
lastupdated: "2026-04-28"

keywords:

subcollection: logs-router

---

{{site.data.keyword.attribute-definition-list}}


# Managing {{site.data.keyword.logs_full_notm}} targets
{: #target_icl}

You can manage {{site.data.keyword.logs_full_notm}} targets in your account by using the {{site.data.keyword.logs_routing_full_notm}} UI, the {{site.data.keyword.logs_routing_full_notm}} CLI, the {{site.data.keyword.logs_routing_full_notm}} REST API, and Terraform scripts. A target is a resource where you can collect platform logs.
{: shortdesc}

For more information on {{site.data.keyword.logs_routing_full_notm}} targets, see [Targets](/docs/logs-router?topic=logs-router-target).


## IAM Access
{: #target_icl_iam_access}

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
{: #target_icl_auth_opts}

When writing to a {{site.data.keyword.logs_full_notm}} target, you must configure a service-to-service (S2S) authorization between {{site.data.keyword.logs_routing_full_notm}} and {{site.data.keyword. logs_full_notm}}.
{: note}

Choose 1 of the following options:

- [Configuring S2S authorization using the UI](/docs/logs-router?topic=logs-router-iam-service-auth-logs-routing&interface=ui)

- [Configuring S2S authorization using the CLI](/docs/logs-router?topic=logs-router-iam-service-auth-logs-routing&interface=cli)

- [Configuring S2S authorization using the API](/docs/logs-router?topic=logs-router-iam-service-auth-logs-routing&interface=api)

- [Configuring S2S authorization using Terraform](/docs/logs-router?topic=logs-router-iam-service-auth-logs-routing&interface=terraform)




## CLI prerequisites
{: #target_icl_prereqs_cli}
{: cli}

Before you use the CLI to manage targets, complete the following steps:

1. [Install the {{site.data.keyword.cloud_notm}} CLI](/docs/cli?topic=cli-install-ibmcloud-cli).

2. [Install the {{site.data.keyword.logs_routing_full_notm}} CLI](/docs/logs-router?topic=logs-router-logs-router-cli-config).

3. Log in to {{site.data.keyword.cloud_notm}}. Run the following command: [ibmcloud login](/docs/cli?topic=cli-ibmcloud_cli#ibmcloud_login)



## Creating an {{site.data.keyword.logs_full_notm}} target using the CLI
{: #target_icl_create_cli}
{: cli}

Use this command to create a {{site.data.keyword.logs_full_notm}} target:

```sh
 ibmcloud logs-router target create --name TARGET_NAME  --destination-crn CLOUD_LOGS_TARGET_CRN [--region REGION] [--managed-by MANAGED-BY] [--output FORMAT]
```
{: pre}

Where

`--region REGION` | `-r REGION`
:   Name of the region, for example, `us-south` or `eu-gb`. If not specified, the region logged into, or targeted, will be used.

    Include this field if you want to create a target in a different region other than the one you are connected.

    The maximum length is 1000 characters. The minimum length is 3 characters.

`--name TARGET_NAME`
:   The name to be given to the target.

    The name must be 1000 characters or less, and cannot include any special characters other than (space), `-`, `.`, `_`, `:`.

    The maximum length is 1000 characters. The minimum length is 1 character.

    Do not include any personal identifying information (PII) in any resource names.
    {: important}

`--destination-crn CLOUD_LOGS_TARGET_CRN`
:   The CRN of the {{site.data.keyword.logs_full_notm}} instance.

    The maximum length is 1000 characters. The minimum length is 3 characters.

`--managed-by MANAGED-BY`
:   Identifies who manages this target.

    If set to `enterprise`, the target becomes enterprise-managed and can only be modified by identities authorized for the enterprise actions.

    If omitted or set to `account`, the target is managed by the child account.

    This value is immutable after creation.{: attention}

    Valid values are: `enterprise`, `account`.

`--output FORMAT`
:   Currently supported format is JSON. If specified, output will be returned in JSON format.  If `JSON` is not specified, output will be returned in a tabular format.

`help` | `--help` | `-h`
:   List options available for the command.

### Example
{: #target_icl_create_cli_sample}

The following is an example using the `ibmcloud logs-router target create --name my-target --destination-crn crn:v1:bluemix:public:logs:global:a/xxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxx:: --managed-by account` command.

This example shows an example successful target creation.
{: note}

```text
OK
Target
Name:                    my-target
ID:                      000000000-00000000-0000-0000-00000000
CRN:                     crn:v1:bluemix:public:logs-router:us-south:a/xxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxx
Region:                  us-south
Type:                    cloud_logs
Cloud Logs Target CRN:   crn:v1:bluemix:public:logs:global:a/xxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxx:
Write Status:            success
CreatedAt:               2026-04-12T19:03:35.427Z
UpdatedAt:               2026-04-12T19:03:35.427Z
```
{: screen}


## Updating a {{site.data.keyword.logs_full_notm}} target using the CLI
{: #target_icl_update_cli}
{: cli}

Use this command to update an {{site.data.keyword.logs_full_notm}} target for an {{site.data.keyword.logs_routing_full_notm}} region.

Any specified value that is different from when the target was originally created will be updated to the value specified in the command.

```sh
ibmcloud logs-router target update --id TARGET [--name TARGET_NAME] --destination-crn CLOUD_LOGS_TARGET_CRN]]] [--output FORMAT]
```
{: pre}

where

`--id TARGET`
:   The 4 UUID that uniquely identifies the target.

    The maximum length is 1028 characters. The minimum length is 24 characters.

`--name TARGET_NAME`
:   The name to be given to the target.

    The name must be 1000 characters or less, and cannot include any special characters other than (space), `-`, `.`, `_`, `:`'.

    The maximum length is 1000 characters. The minimum length is 1 character.

    Do not include any personal identifying information (PII) in any resource names.
    {: important}

`--destination-crn CLOUD_LOGS_TARGET_CRN`
:   The CRN of the {{site.data.keyword.logs_full_notm}} instance.

`--output FORMAT`
:   Currently supported format is JSON. If specified, output will be returned in JSON format.  If `JSON` is not specified, output will be returned in a tabular format.

`help` | `--help` | `-h`
:   List options available for the command.

### Example
{: #target_icl_update_cli_sample}

The following is an example using the `ibmcloud logs-router target update --target my-target --name new-target-name` command.

```text
OK
Target
Name:                    new-target-name
ID:                      000000000-00000000-0000-0000-00000000
CRN:                     crn:v1:bluemix:public:logs-router:us-south:a/xxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxx
Region:                  us-south
Type:                    cloud_logs
Cloud Logs Target CRN:   crn:v1:bluemix:public:logs:global:a/xxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxx:
Write Status:            success
Created:                 2024-05-31T19:03:35.427Z
Updated:                 2024-05-31T19:03:35.427Z
```
{: screen}

## Deleting a target using the CLI
{: #target_icl_delete_cli}
{: cli}

Use this command to delete a target.

```sh
ibmcloud logs-router target delete --id TARGET [--force]
```
{: pre}

where

`--id TARGET`
:   The 4 UUID that uniquely identifies the target.

    The maximum length is 1028 characters. The minimum length is 24 characters.

`--force` | `-f`
:   Will delete the target without providing the user with any additional prompt.

`help` | `--help` | `-h`
:   List options available for the command.

### Example
{: #target_icl_delete_cli_sample}

The following is an example using the `ibmcloud logs-router target delete --id xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx` command.

```text
Are you sure you want to remove the target with target ID xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx? [y/N]>y
OK
Target with target ID xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx was successfully removed.
```
{: screen}

The following is an example using the `ibmcloud logs-router target delete --id xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx -force` command.

This example shows a failed command where the specified target could not be found.
{: note}

```text
Are you sure you want to remove the Target bearing Target ID 33333333-3333-3333-3333-333333333333? [y/N]> y
FAILED
Something went wrong. Error:
 Status Code:  404
 Incident ID:  67a33257-d5a4-46ec-94d9-14eb70e94f3d
 Code:         not_found
 Message:      The target id specified in `target_id` field is not found.
```
{: screen}


## Validating and viewing details of a target by using the CLI
{: #target_icl_get_cli}
{: cli}

Use this command to get information about a target for an {{site.data.keyword.logs_routing_full_notm}} region.

```sh
ibmcloud logs-router target get --id TARGET [--output FORMAT]
```
{: pre}

Where

`--id TARGET`
:   The 4 UUID that uniquely identifies the target.

    The maximum length is 1028 characters. The minimum length is 24 characters.

`--output FORMAT`
:   Currently supported format is JSON. If specified, output will be returned in JSON format.  If `JSON` is not specified, output will be returned in a tabular format.

`help` | `--help` | `-h`
:   List options available for the command.

The response includes information about the status of the target in the `write_status` response object. This object includes the following information:
- `Write Status` information. Valid values are: `success` or `failed`.
- `Last Failure` date-time information of the timestamp of the last failure.
- `Reason for last failure` information with a detailed description of the cause of the failure.

### Example
{: #target_icl_get_cli_sample}

The following is an example using the `ibmcloud logs-router target get --id new-target-name` command showing an {{site.data.keyword.logs_full_notm}} target.

```text
Target
Name:                       new-target-name
ID:                         xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
CRN:                        crn:v1:bluemix:public:atracker:us-south:a/xxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxx
Type:                       cloud_logs
Cloud Logs Target CRN:      crn:v1:bluemix:public:logs:global:a/xxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxx:
Service to Service Enabled: true
Write Status:               success
Created:                    2024-06-02T16:04:15.174Z
Updated:                    2024-06-05T17:49:56.452Z
```
{: screen}

## Listing all targets in a region
{: #target_icl_list_cli}
{: cli}

Use this command to list the configured targets for an {{site.data.keyword.logs_routing_full_notm}} region.

```sh
ibmcloud logs-router target list [--output FORMAT]
```
{: pre}

Where

`--output FORMAT`
:   Currently supported format is JSON. If specified, output will be returned in JSON format.  If `JSON` is not specified, output will be returned in a tabular format.

`help` | `--help` | `-h`
:   List options available for the command.

### Example
{: #target_icl_list_cli_sample}

The following is an example using the **`ibmcloud logs-router target list`** command.

```text
Name                       ID                                     Region     Type                Created
target-01                  xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx   us-south   cloud_logs          2020-11-18T03:52:08.603Z
```
{: screen}


## API targets and actions
{: #target_icl_api}
{: api}

The following table lists the actions that you can run to manage targets:

| Action                     | REST API Method  | API_URL                                          |
|----------------------------|------------------|--------------------------------------------------|
| Create a target            | `POST`           | `<ENDPOINT>/v3/targets`              |
| Update a target            | `PATCH`          | `<ENDPOINT>/v3/targets/<TARGET_ID>`  |
| Delete a target            | `DELETE`         | `<ENDPOINT>/v3/targets/<TARGET_ID>`  |
| Read a target              | `GET`            | `<ENDPOINT>/v3/targets/<TARGET_ID>`  |
| List all targets           | `GET`            | `<ENDPOINT>/v3/targets`              |
| Validate a target          | `GET`            | `<ENDPOINT>/v3/targets`              |
{: caption="Target actions by using the {{site.data.keyword.logs_routing_full_notm}} REST API" caption-side="top"}

You can use private and public endpoints to manage targets. For more information about the list of `ENDPOINTS` that are available, see [Endpoints](/docs/logs-router?topic=logs-router-endpoints).

- You can manage targets from the private network using an API endpoint with the following format: `https://private.REGION.logs-router.cloud.ibm.com`
- You can manage targets from the public network using an API endpoint with the following format: `https://REGION.logs-router.cloud.ibm.com`

- You can disable the public endpoints by updating the account settings. For more information, see [Configuring target and region settings](/docs/logs-router?topic=logs-router-settings).

For more information about the REST API, see [{{site.data.keyword.logs_routing_full_notm}} REST API v3](/apidocs/logs-router-service-api/logs-router-v3).
{: note}


## API prerequisites
{: #target_icl_api_prereqs}
{: api}

To make API calls to manage targets, complete the following steps:
1. Get an IAM access token. For more information, see [Retrieving IAM access tokens](/docs/logs-router?topic=logs-router-retrieve-access-token&interface=api).
2. Identify the API endpoint in the region where you plan to configure or manage a target. For more information, see [Endpoints](/docs/logs-router?topic=logs-router-endpoints).

## Creating an {{site.data.keyword.logs_full_notm}} target using the API
{: #target_icl_api_create}
{: api}

You can create a target to configure a destination where you can route platform logs.
- You can send your platform logs from all regions to a single target, different targets or multiple targets.
- You can define up to 16 targets per account.
- You can manage targets at enterprise level setting the `managed by` parameter to **enterprise**. This value is immutable and cannot be changed after the target is created.


You can use the following cURL command to create a {{site.data.keyword.logs_full_notm}} target:

```shell
curl -X POST  <ENDPOINT>/v3/targets   -H "Authorization:  $ACCESS_TOKEN"   -H "content-type: application/json"   -d '{
    "name": "TARGET_NAME",
    "destination_crn": "CLOUD_LOGS_CRN",
    "region": "eu-gb",
    "managed_by": "account"
  }'
```
{: codeblock}

Where

`<ENDPOINT>`
:   Endpoint is the API endpoint in the region where you plan to configure or manage a target. For more information, see [Endpoints](/docs/logs-router?topic=logs-router-endpoints).

`TARGET_NAME`
:   The name to be given to the target.

    The name must be 1000 characters or less, and cannot include any special characters other than (space), `-`, `.`, `_`, `:`.

    The maximum length is 1000 characters. The minimum length is 1 character.

    Do not include any personal identifying information (PII) in any resource names.
    {: important}

`CLOUD_LOGS_TARGET_CRN`
:   The CRN of the {{site.data.keyword.logs_full_notm}} instance.

    The maximum length is 1000 characters. The minimum length is 3 characters.

`REGION`
:   Name of the region, for example, `us-south` or `eu-gb`. If not specified, the region logged into, or targeted, will be used.

    Include region if you want to create a target in a different region other than the one you are connected.

    The maximum length is 1000 characters. The minimum length is 3 characters.

`MANAGED-BY`
:   Identifies who manages this target.

    If set to `enterprise`, the target becomes enterprise-managed and can only be modified by identities authorized for the enterprise actions.

    If omitted or set to `account`, the target is managed by the child account.

    This value is immutable after creation.{: attention}

    Valid values are: `enterprise`, `account`.

For example, you can use the following cURL request to create a target in Dallas:

```shell
curl -X POST   https://api.us-south.logs-router.cloud.ibm.com/v3/targets   -H "Authorization: $ACCESS_TOKEN"   -H "content-type: application/json"   -d '{
    "name": "a-cloud-logs-target-us-south",
    "destination_crn": "crn:v1:bluemix:public:logs:us-south:a/0be5ad401ae913d8ff665d92680664ed:22222222-2222-2222-2222-222222222222::"
  }'
```
{: screen}

In the response, you get information about the target such as the `id`, that indicates the GUID of the target, and the `crn`, that indicates the CRN of the target.


## Updating a {{site.data.keyword.logs_full_notm}} target using the API
{: #target_icl_api_update}
{: api}

When you update an {{site.data.keyword.logs_full_notm}} target, you must include the target information in the data section of the request.
- You must pass all fields.
- Update the fields that need to be changed.

You can use the following cURL command to update a target:

```shell
curl -X PATCH  <ENDPOINT>/v3/targets/TARGET_ID  -H "Authorization:  $ACCESS_TOKEN"   -H "content-type: application/json"   -d '{
    "name": "TARGET_NAME",
    "destination_crn": "CLOUD_LOGS_CRN"
  }'
```
{: codeblock}

Where

`<ENDPOINT>`
:   Endpoint is the API endpoint in the region where you plan to configure or manage a target. For more information, see [Endpoints](/docs/logs-router?topic=logs-router-endpoints).


`TARGET_ID`
:   The v4 UUID that uniquely identifies the target.

`TARGET_NAME`
:   The name to be given to the target.

    The name must be 1000 characters or less, and cannot include any special characters other than (space), `-`, `.`, `_`, `:`.

    The maximum length is 1000 characters. The minimum length is 1 character.

    Do not include any personal identifying information (PII) in any resource names.
    {: important}

`CLOUD_LOGS_TARGET_CRN`
:   The CRN of the {{site.data.keyword.logs_full_notm}} instance.

    The maximum length is 1000 characters. The minimum length is 3 characters.

For example, you can use the following cURL request to update a target in Dallas:

```shell
curl -X PATCH   https://api.us-south.logs-router.cloud.ibm.com/v3/targets/TARGET_ID   -H "Authorization: $ACCESS_TOKEN"   -H "content-type: application/json"   -d '{
    "name": "a-cloud-logs-target-modified",
    "destination_crn": "crn:v1:bluemix:public:logs:us-south:a/0be5ad401ae913d8ff665d92680664ed:33333333-3333-3333-3333-333333333333::"
  }'
```
{: screen}



## Deleting a target using the API
{: #target_icl_api_delete}
{: api}

You can use the following cURL command to delete a target:

```text
curl -X DELETE <ENDPOINT>/v3/targets/<TARGET_ID> -H "Authorization:  $ACCESS_TOKEN" -H "content-type: application/json"
```
{: codeblock}

Where

- `<ENDPOINT>` is the API endpoint in the region where you plan to configure or manage a target. For more information, see [Endpoints](/docs/logs-router?topic=logs-router-endpoints).
- `<TARGET_ID>` is the v4 UUID that uniquely identifies the target.


For example, you can use the following cURL request to delete a target in US-South with the ID `00000000-0000-0000-0000-000000000000`:

```shell
curl -X DELETE https://api.us-south.logs-router.cloud.ibm.com/v3/targets/00000000-0000-0000-0000-000000000000 -H "Authorization: Bearer <IAM_TOKEN> -H "content-type: application/json""
```
{: screen}

In the response, you get an empty result if the deletion was successful:

```json
{}
```
{: screen}



## Validating and viewing details of a target by using the API
{: #target_icl_api_view}
{: api}

You can use the following cURL command to view the configuration details of 1 target:

```shell
curl -X GET <ENDPOINT>/v3/targets/<TARGET_ID> -H "Authorization: $ACCESS_TOKEN" -H "content-type: application/json"
```
{: codeblock}

Where

- `<ENDPOINT>` is the API endpoint in the region where you plan to configure or manage a target. For more information, see [Endpoints](/docs/logs-router?topic=logs-router-endpoints).
- `<TARGET_ID>` is the v4 UUID that uniquely identifies the target.

The response includes information about the status of the target in the `write_status` response object. This object includes the following information:
- `status` information. Valid values are: `success` or `failed`.
- `last_failure` date-time information of the timestamp of the last failure.
- `reason_for_last_failure` information with a detailed description of the cause of the failure.


For example, you can run the following cURL request to get information about a target with the ID `00000000-0000-0000-0000-000000000000`:

```shell
curl -X GET https://api.us-south.logs-router.cloud.ibm.com/v3/targets/00000000-0000-0000-0000-000000000000 -H "Authorization: Bearer <IAM_TOKEN> -H "content-type: application/json""
```
{: screen}

The response looks as follows:

```text
{
  "name": "cloud-logs-target",
  "id": "00000000-0000-0000-0000-000000000000",
  "crn": "crn:v1:bluemix:public:logs-router:us-south:a/4329073d16d2f3663f74bfa955259139::target:00000000-0000-0000-0000-000000000000",
  "destination_crn": "crn:v1:bluemix:public:logs:us-south:a/0be5ad401ae913d8ff665d92680664ed:22222222-2222-2222-2222-222222222222::",
  "target_type": "cloud_logs",
  "write_status": {
    "status": "success"
  },
  "created_at": "2026-02-01T19:39:38.174Z",
  "updated_at": "2026-02-01T19:39:38.174Z",
  "managed_by": "enterprise"
}
```
{: screen}

## Listing all targets using the API
{: #target_icl_api_list}
{: api}

You can use the following cURL command to view all targets:

```shell
curl -X GET <ENDPOINT>/v3/targets -H "Authorization: $ACCESS_TOKEN" -H "content-type: application/json"
```
{: codeblock}

Where

- `<ENDPOINT>` is the API endpoint in the region where you plan to configure or manage a target. For more information, see [Endpoints](/docs/logs-router?topic=logs-router-endpoints).


For example, you can run the following cURL request to get information about the targets that are defined in Dallas:

```shell
curl -X GET https://private.us-south.logs-router.cloud.ibm.com/v3/targets -H "Authorization:  $ACCESS_TOKEN" -H "content-type: application/json"
```
{: screen}



## HTTP response codes
{: #target_icl_api_rc}
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


## Creating a {{site.data.keyword.logs_full_notm}} target using the UI
{: #target_icl_ui_create}
{: ui}

Only resources in your account are listed and selectable. To specify a resource in a different account, select **Specify CRN** under **Choose destination**.
{: important}

Complete the following steps to create a target:

1. [Log in to your {{site.data.keyword.cloud_notm}} account](https://cloud.ibm.com/login){: external}.
2. Click the **Menu** icon ![Menu icon](../icons/icon_hamburger.svg) &gt; **Observability**.
3. Select **Logging > Routing**.
4. In the *Targets* section, click **Create** to open the create panel.
5. In the *Choose destination*, pick **Search by instance** or **Specify CRN**.

    Select **Search by instance** to choose an {{site.data.keyword.logs_full_notm}} instance from the table that is available in the account

    Select **Specify CRN** to add the {{site.data.keyword.logs_full_notm}} instance CRN of an instance that is located in a different account.

    A service authorization is required to allow {{site.data.keyword.logs_routing_full_notm}} to communicate with {{site.data.keyword.logs_full_notm}}.

6. In the *target details* section, complete the following tasks:

    Enter the target name.

    Enable the toggle from **Not set** to automatically set your new target as a default target in your {{site.data.keyword.logs_routing_full_notm}} settings.

7. Click **Create target**.


## Updating a {{site.data.keyword.logs_full_notm}} target using the UI
{: #target_icl_ui_update}
{: ui}

Only resources in your account are listed and selectable. To specify a resource in a different account, select **Specify CRN** under **Choose destination**.
{: important}

Complete the following steps:

1. [Log in to your {{site.data.keyword.cloud_notm}} account](https://cloud.ibm.com/login){: external}.
2. Click the **Menu** icon ![Menu icon](../icons/icon_hamburger.svg) &gt; **Observability**.
3. Select **Logging > Routing**.
4. In the *Targets* section, determine which target to update and click the ![Actions icon](../icons/action-menu-icon.svg "Actions").
5. Click **Edit** to open the update panel.
6. In the Update panel, click **Edit** to update your target's name, CRN to change the {{site.data.keyword.logs_full_notm}} instance associated with your target, or both. You can also toggle **Default target** to add or remove your target as a default target in your {{site.data.keyword.logs_routing_full_notm}} settings.
7. Click **Save** to update your target.



## Deleting a target using the UI
{: #target_icl_ui_delete}
{: ui}

You cannot delete an {{site.data.keyword.logs_routing_full_notm}} target if it is used in a route or as a default target setting.
{: important}

1. [Log in to your {{site.data.keyword.cloud_notm}} account](https://cloud.ibm.com/login){: external}.
2. Click the **Menu** icon ![Menu icon](../icons/icon_hamburger.svg) &gt; **Observability**.
3. Select **Logging > Routing**.
4. In the *Targets* section, determine which target to update and click the ![Actions icon](../icons/action-menu-icon.svg "Actions").
7. Select **Delete** and then click **Delete** in the confirmation panel.


## Listing all targets in a region using the UI
{: #target_icl_ui_list}
{: ui}

1. [Log in to your {{site.data.keyword.cloud_notm}} account](https://cloud.ibm.com/login){: external}.
2. Click the **Menu** icon ![Menu icon](../icons/icon_hamburger.svg) &gt; **Observability**.
3. Select **Logging > Routing**.

You can see the list of targets that are defined in the account. The following information is included per target:
- Target name
- Target type
- Destination name
- Destination region
- Routes define if the target is used in any routes.
- Target status:

    - **Configured**: The target is working as expected.
    - **Not receiving**: The target is miscosfigured and events will not be routed to the destination. Update your target details or destination to fix the target configuration or delete the target if it is no longer needed.
