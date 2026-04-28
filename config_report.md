---

copyright:
  years:  2023, 2026
lastupdated: "2026-04-28"

keywords:

subcollection: logs-router

---

{{site.data.keyword.attribute-definition-list}}



# Creating a report of the {{site.data.keyword.logs_routing_full_notm}} configuration
{: #config_report}

After configuring your {{site.data.keyword.logs_routing_full_notm}} resources for your account, you should save a copy of your configuration for reference and backup purposes.
{: shortdesc}

## Prereqs
{: #config_report_prereqs}
{: cli}

Before you use the CLI to save your {{site.data.keyword.logs_routing_full_notm}} resource data, you must do the following:

1. [Install the {{site.data.keyword.cloud_notm}} CLI](/docs/cli?topic=cli-install-ibmcloud-cli).

2. [Install the {{site.data.keyword.logs_routing_full_notm}} CLI](/docs/logs-router?topic=logs-router-logs-router-cli-config).

3. Ensure you have the [correct IAM permissions to configure {{site.data.keyword.logs_routing_full_notm}} routes.](/docs/logs-router?topic=logs-router-iam)

4. Log in to {{site.data.keyword.cloud_notm}}. Run the following command: [ibmcloud login](/docs/cli?topic=cli-ibmcloud_cli#ibmcloud_login).

5. Make sure you have configured your [settings](/docs/logs-router?topic=logs-router-settings), [targets](/docs/logs-router?topic=logs-router-target-manage&interface=cli) and [routes](/docs/logs-router?topic=logs-router-route_rules_definitions).


## Saving a copy of your account settings
{: #config_report_save_settings_cli}
{: cli}

Run the following command to save a copy of your account settings to a file.

```text
ibmcloud logs-router setting get --output JSON > ACCOUNT_settings.json
```
{: pre}

Where `ACCOUNT` is a unique indicator for your account.

## Saving a copy of your route configuration
{: #config_report_save_route_cli}
{: cli}

Run the following command to save a copy of your route configuration to a file.

```text
ibmcloud logs-router route ls --output JSON > ACCOUNT_routes.json
```
{: pre}

Where `ACCOUNT` is a unique indicator for your account.

## Saving a copy of your target configuration
{: #config_report_save_target_cli}
{: cli}

Run the following command to save a copy of your target configuration to a file.

```text
ibmcloud logs-router target ls --output JSON > ACCOUNT_targets.json
```
{: pre}

Where `ACCOUNT` is a unique indicator for your account.



## Getting settings using the API
{: #config_report_save_settings_api}
{: api}

You can use the following cURL command to get existing settings information:

```shell
curl -X GET  ENDPOINT/api/v3/settings   -H "Authorization:  $ACCESS_TOKEN" > ACCOUNT_settings.json
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
