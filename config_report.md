---

copyright:
  years:  2023, 2026
lastupdated: "2026-04-29"

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

2. [Install the {{site.data.keyword.logs_routing_full_notm}} CLI](/docs/logs-router?topic=logs-router-logs-router-cli).

3. Ensure you have the [correct IAM permissions to configure {{site.data.keyword.logs_routing_full_notm}} routes.](/docs/logs-router?topic=logs-router-iam)

4. Log in to {{site.data.keyword.cloud_notm}}. Run the following command: [ibmcloud login](/docs/cli?topic=cli-ibmcloud_cli#ibmcloud_login).

5. Make sure you have configured your [settings](/docs/logs-router?topic=logs-router-settings), [targets](/docs/logs-router?topic=logs-router-target_icl) and [routes](/docs/logs-router?topic=logs-router-route-manage).


## Saving a copy of your account settings
{: #config_report_save_settings_cli}
{: cli}

Run the following command to save a copy of your account settings to a file.

```text
ibmcloud logs-router setting get --output JSON > ACCOUNT_settings.json
```
{: pre}

Where `ACCOUNT` is a unique indicator for your account.

## Saving a copy of your route configurations
{: #config_report_save_route_cli}
{: cli}

Run the following command to save a copy of your route configuration to a file.

```text
ibmcloud logs-router route list --output JSON > ACCOUNT_routes.json
```
{: pre}

Where `ACCOUNT` is a unique indicator for your account.

## Saving a copy of your target configurations
{: #config_report_save_target_cli}
{: cli}

Run the following command to save a copy of your target configuration to a file.

```text
ibmcloud logs-router target list --output JSON > ACCOUNT_targets.json
```
{: pre}

Where `ACCOUNT` is a unique indicator for your account.


## Saving a copy of your account settings
{: #config_report_save_settings_api}
{: api}

You can use the following cURL command to save a copy of your account settings to a file.

```shell
curl -X GET https://api.<REGION>.logs-router.cloud.ibm.com/v3/settings -H "Authorization:  $ACCESS_TOKEN" > ACCOUNT_settings.json
```
{: codeblock}

Where:

`REGION`
:   Is the primary metadata region for your account.

## Saving a copy of your route configurations
{: #config_report_save_route_api}
{: api}

You can use the following cURL command to save a copy of your route configurations to a file.

```shell
curl -X GET https://api.<REGION>.logs-router.cloud.ibm.com/v3/routes -H "Authorization:  $ACCESS_TOKEN" > ACCOUNT_routes.json
```
{: codeblock}

Where:

`REGION`
:   Is the primary metadata region for your account.

## Saving a copy of your target configurations

{: #config_report_save_target_api}
{: api}

You can use the following cURL command to save a copy of your target configurations to a file.

```shell
curl -X GET https://api.<REGION>.logs-router.cloud.ibm.com/v3/targets -H "Authorization:  $ACCESS_TOKEN" > ACCOUNT_targets.json
```
{: codeblock}

Where:

`REGION`
:   Is the primary metadata region for your account.
