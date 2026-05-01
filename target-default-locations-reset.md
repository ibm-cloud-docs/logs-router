---

copyright:
  years:  2023, 2026
lastupdated: "2026-04-28"

keywords:

subcollection: logs-router

---

{{site.data.keyword.attribute-definition-list}}



# Reseting the permitted target regions in the account
{: #target-default-locations-reset}

To reset the {{site.data.keyword.logs_routing_full_notm}} account's permitted target regions, you must delete the permitted target regions from the account configuration.
{: shortdesc}



## Resetting target regions using the UI
{: #reset_permitted_targets_ui}
{: ui}

You can remove all permitted target regions using the {{site.data.keyword.logs_routing_full_notm}} UI by updating your account settings and removing all permitted target regions. For more information, see [Configuring account settings](/docs/logs-router?topic=logs-router-settings&interface=ui).


## Prereqs
{: #target-default-locations-reset-prereqs}
{: cli}

1. [Install the {{site.data.keyword.cloud_notm}} CLI](/docs/cli?topic=cli-install-ibmcloud-cli).

2. [Install the {{site.data.keyword.logs_routing_full_notm}} CLI](/docs/logs-router?topic=logs-router-logs-router-cli-config).

3. Ensure you have the [correct IAM permissions to configure {{site.data.keyword.logs_routing_full_notm}} settings](/docs/logs-router?topic=logs-router-iam).

4. Log in to {{site.data.keyword.cloud_notm}} by running the following command: [ibmcloud login](/docs/cli?topic=cli-ibmcloud_cli#ibmcloud_login)



## Check your current account settings
{: #target-acct-reset-step1}
{: cli}
{: step}

To check your account's configuration and find out if there are default targets configured, run the following command:

```text
ibmcloud logs-router setting get
```
{: pre}

The setting **Permitted target regions** lists the locations in the account that have been configured as valid locations where targets can be created.

## Reset the default targets
{: #target-acct-reset-step2}
{: cli}
{: step}

Run the following command to reset the account's default targets:

```sh
ibmcloud logs-router setting update --default-targets '[{}]'
```
{: pre}
