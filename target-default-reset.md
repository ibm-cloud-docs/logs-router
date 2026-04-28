---

copyright:
  years:  2023, 2026
lastupdated: "2026-04-28"

keywords:

subcollection: logs-router

---

{{site.data.keyword.attribute-definition-list}}



# Reseting the default targets in the account
{: #target-default-reset}

To reset the {{site.data.keyword.logs_router_full_notm}} account's default targets, you must delete any default targets from the account configuration.
{: shortdesc}



## Removing all default targets using the UI
{: #remove_default_targets_ui}
{: ui}

You can remove all default targets using the {{site.data.keyword.logs_router_full_notm}} UI by updating your account settings and deleting the default targets. For more information, see [Configuring account settings](/docs/logs-router?topic=logs-router-settings&interface=ui).


## Prereqs
{: #target-default-reset-prereqs}
{: cli}


1. [Install the {{site.data.keyword.cloud_notm}} CLI](/docs/cli?topic=cli-install-ibmcloud-cli).

2. [Install the {{site.data.keyword.logs_router_full_notm}} CLI](/docs/logs-router?topic=logs-router-logs-router-cli-config).

3. Ensure you have the [correct IAM permissions to configure {{site.data.keyword.logs_router_full_notm}} settings.](/docs/logs-router?topic=logs-router-iam)

4. Log in to {{site.data.keyword.cloud_notm}} by running the following command: [ibmcloud login](/docs/cli?topic=cli-ibmcloud_cli#ibmcloud_login)



## Identify targets
{: #target-default-reset-step1}
{: cli}
{: step}

Get the list of targets that are defined in the account. Run the following command:

```sh
ibmcloud logs-router target list
```
{: pre}


## Check your current account settings
{: #target-default-reset-step2}
{: cli}
{: step}

To check your account's configuration and find out if there are default targets configured, run the following command:

```sh
ibmcloud logs-router setting get
```
{: pre}

The setting **Default targets** lists any targets in the account that have been configured as default targets.

## Reset the default targets
{: #target-default-reset-step3}
{: cli}
{: step}

Run the following command to reset the account's default targets:

```sh
ibmcloud logs-router setting update --default-targets '[{}]'
```
{: pre}
