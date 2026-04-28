---

copyright:
  years:  2023, 2026
lastupdated: "2026-04-28"

keywords:

subcollection: logs-router

---

{{site.data.keyword.attribute-definition-list}}

# Configuring account settings
{: #settings}

You can configure {{site.data.keyword.logs_routing_full_notm}} account settings to define where and how platform logs are collected, routed, and managed in your account. You can do it through the UI, or by using the CLI, the REST API V3, or Terraform scripts.
{: shortdesc}

Before you disable public endpoints by setting `--private-api-endpoint-only TRUE`, make sure your account has access to the private endpoint.  You can do this by running the command `ibmcloud account show`.  If `VRF Enabled` is `true` and `Service Endpoint Enabled` is `true` then you have access to the private endpoint. If you do not have access to the private endpoint, you will be unable to re-enable the public endpoint since private endpoint access is required to re-enable the public endpoint.
{: attention}

For more information, see [About account settings](/docs/logs-router?topic=logs-router-settings-about&interface=api).


## IAM permissions
{: #settings-access}

Ensure you have the [correct IAM permissions to configure {{site.data.keyword.logs_routing_full_notm}} settings.](/docs/logs-router?topic=logs-router-iam)

## Getting details of the account settings by using the UI
{: #settings-get-ui}
{: ui}

Complete the following steps to view the account settings for {{site.data.keyword.logs_routing_full_notm}} by using the UI:

1. [Log in to your {{site.data.keyword.cloud_notm}} account](https://cloud.ibm.com/login){: external}.

2. Click the **Menu** icon ![Menu icon](../icons/icon_hamburger.svg) &gt; **Observability**.

3. Click **Logging > Routing**.

4. Click the **Settings** tab.

On this page you can view the following settings:

- **Metadata location**: Displays primary metadata region and backup metadata region.

- **Permitted target regions**. Displays the targets regions where platform logs can be sent.

- **Default targets**: Displays the configured default targets.

- **Public endpoints**: Displays if public endpoints are enabled. When disabled, the {{site.data.keyword.logs_routing_full_notm}} UI cannot be accessed.

- **Reports**: Displays the configurations in JSON format.

- **API version**

## Updating the account settings using the UI
{: #settings-update-ui}
{: ui}

1. [Log in to your {{site.data.keyword.cloud_notm}} account](https://cloud.ibm.com/login){: external}.

2. Click the **Menu** icon ![Menu icon](../icons/icon_hamburger.svg) &gt; **Observability**.

3. Click **Logging >Routing**.

4. Click the **Settings** tab.

5. Click **Edit** next to the setting to be changed. You can modify the following settings:

- **Metadata location**: Select your desired primary metadata region and backup metadata region.

- **Permitted target regions**: Select the region where targets can be created. If no regions are selected, then all regions can receive platform logs.

- **Default targets**: Select the target that will be used by default when routing rules do not exist or are not matched.


## CLI prerequisites
{: #settings-prereqs-cli}
{: cli}

Before you use the the CLI to manage {{site.data.keyword.logs_routing_full_notm}} account settings, [Install the {{site.data.keyword.logs_routing_full_notm}} CLI](/docs/logs-router?topic=logs-router-logs-router-cli-config).

Check that you have IAM permissions to read, update, or both the {{site.data.keyword.logs_routing_full_notm}} account settings.


## Getting details of the account settings by using the CLI
{: #settings-get-using-cli}
{: cli}

Use this command to get the settings for the {{site.data.keyword.logs_routing_full_notm}} account configurations.

```sh
ibmcloud logs-router setting get [--output FORMAT]
```
{: pre}

### Command options
{: #settings-get-options-cli}

`--output FORMAT`
:   If `JSON` is specified, output will be returned in JSON format.  If `JSON` is not specified, output will be returned in a tabular format.

`help` | `--help` | `-h`
:   List options available for the command.


### Example
{: #settings-get-example}

The following is an example where no default or permitted targets have been set.

```text
Default Targets             -
Permitted Target Regions    -
Primary Metadata Region
Backup Metadata Region
Private Api Endpoint Only   false
Api Version                 3
```
{: codeblock}

## Updating the account settings using the CLI
{: #settings-update-using-cli}
{: cli}

Use this command to modify current account settings such as the default targets, permitted target regions, and the primary metadata region.

```sh
ibmcloud logs-router setting update [--default-targets DEFAULT-TARGETS | @DEFAULT-TARGETS-FILE] [--permitted-target-regions PERMITTED-TARGET-REGIONS] [--primary-metadata-region PRIMARY-METADATA-REGION] [--backup-metadata-region BACKUP-METADATA-REGION] [--private-api-endpoint-only=PRIVATE-API-ENDPOINT-ONLY] [--output FORMAT] [--force]
```
{: pre}

### Command options
{: #settings-update-options-cli}

`primary-metadata-region`
:   Specify the REGION where the metadata associated with route and target definitions is stored.

    The maximum length is 256 characters. The minimum length is 3 characters.

`backup-metadata-region`
:   Specify the REGION where the metadata associated with route and target definitions is stored as a backup.

    The maximum length is 256 characters. The minimum length is 3 characters.

`private-api-endpoint-only`
:   Specifies whether nor not a private endpoint can be used. If `TRUE` only a private endpoint can be used.

`default-targets`
:   Is a list of target IDs.  If no routing rules cause platform logs to be sent to other targets, these targets will received the platform logs. TARGETS is a comma-separated list of target IDs.

    The maximum length is 2 items. The minimum length is 0 items.

`permitted-target-regions`
:   Is the list of regions that can be used to define a target. REGIONS is a comma-separated list of regions.

    The maximum length is 16 items. The minimum length is 0 items.

`--output FORMAT`
:   If `JSON` specified, output will be returned in JSON format.  If `JSON` is not specified, output will be returned in a tabular format.

`help` | `--help` | `-h`
:   List options available for the command.

If the update is successful, the current settings will be displayed.



## API prerequsites
{: #settings-prereqs-api}
{: api}

To make API calls to manage settings, complete the following steps:
1. Get an IAM access token. For more information, see [Retrieving IAM access tokens](/docs/logs-router?topic=logs-router-iam-retrieve-token&interface=cli).
2. Identify the API endpoint in the region where you plan to configure or manage settings. For more information, see [Endpoints](/docs/logs-router?topic=logs-router-endpoints).


## Getting details of the account settings by using the API
{: #settings-get-api}
{: api}

You can use the following cURL command to get existing settings information:

```shell
curl -X GET  ENDPOINT/api/v3/settings   -H "Authorization:  $ACCESS_TOKEN"
```
{: codeblock}

Where:

`ENDPOINT`
:   Is the API endpoint in the region where you plan to configure or manage a target. For more information, see [Endpoints](/docs/logs-router?topic=logs-router-endpoints).

For example, you can use the following cURL request to get the account settings information:

```shell
curl -X GET   https://api.<region>.logs-router.cloud.ibm.com/v3/settings   -H "Authorization:  $ACCESS_TOKEN"
```
{: screen}

A response similar to the following is returned:

```json
{
  "default_targets":[],
  "permitted_target_regions":[],
  "primary_metadata_region":"",
  "backup_metadata_region":"",
  "private_api_endpoint_only":false,
  "api_version":3
}
```
{: screen}

Where:

`default_targets`
:   Is a list of target IDs.  If no routing rules cause platform logs to be sent to other targets, these targets will received the platform logs.

`permitted_target_regions`
:   Is the list of regions that can be used to define a target. A maximum of two permitted target regions are allowed.

`primary_metadata_region`
:   Is the region where the metadata associated with route and target definitions is stored.

`backup_metadata_region`
:   Is the region where the metadata associated with route and target definitions is stored as a backup.

`private_api_endpoint_only`
:   Specifies whether nor not a private endpoint can be used.  If `true` only a private endpoint can be used.


## Updating the account settings using the API
{: #settings-update-api}
{: api}

When you update settings, consider the following information:
- You must include the settings information in the data section of the request.
- You must pass the data that is configured for each setting.

    Data that is configured and not included in an update request is deleted.
    {: important}

- Update the data for the fields that need changing.

You can use the following cURL command to update settings:

```shell
curl -X PATCH   https://api.<region>.logs-router.cloud.ibm.com/v3/settings
-H "Authorization: Bearer <IAM_TOKEN>"
-H 'accept: application/json'
-d '{
    "default_targets": [
      {
        "id": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
      }
    ],
    "permitted_target_regions": ["us-south", "us-east"],
    "primary_metadata_region": "us-south",
    "backup_metadata_region": "us-east",
    "private_api_endpoint_only": false
  }'
```
{: codeblock}

Where:

`ENDPOINT`
:    Is the API endpoint in the region where you plan to configure or manage a target. For more information, see [Endpoints](/docs/logs-router?topic=logs-router-endpoints).

`default_targets`
:   Is a list of target IDs.  If no routing rules cause platform logs to be sent to other targets, these targets will received the platform logs.

`permitted_target_regions`
:   Is the list of regions that can be used to define a target. A maximum of two permitted target regions can be specified.

`primary_metadata_region`
:   Is the region where the metadata associated with route and target definitions is stored.

`backup_metadata_region`
:   Is the region where the metadata associated with route and target definitions is stored for backup purposes.

`private_api_endpoint_only`
:   Specifies whether or not a private endpoint can be used.  If `true` only a private endpoint can be used.
