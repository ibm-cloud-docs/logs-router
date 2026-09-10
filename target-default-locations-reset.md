---

copyright:
  years:  2023, 2026
lastupdated: "2026-09-10"

keywords:

subcollection: logs-router

---

{{site.data.keyword.attribute-definition-list}}



# How do I reset the permitted target regions in {{site.data.keyword.logs_routing_full}}
{: #target-default-locations-reset}

To reset the {{site.data.keyword.logs_routing_full_notm}} account's permitted target regions, you must delete the permitted target regions from the account configuration.
{: shortdesc}



## What are the CLI prerequisites to reset the permitted target regions in {{site.data.keyword.logs_routing_full}}
{: #target-default-locations-reset-prereqs}
{: cli}

1. [Install the {{site.data.keyword.cloud_notm}} CLI](/docs/cli?topic=cli-install-ibmcloud-cli).

2. [Install the {{site.data.keyword.logs_routing_full_notm}} CLI](/docs/logs-router?topic=logs-router-logs-router-cli-config).

3. Ensure you have the [correct IAM permissions to configure {{site.data.keyword.logs_routing_full_notm}} settings](/docs/logs-router?topic=logs-router-iam).

4. Log in to {{site.data.keyword.cloud_notm}} by running the following command: [ibmcloud login](/docs/cli?topic=cli-ibmcloud_cli#ibmcloud_login)



## How do I check the current {{site.data.keyword.cloud_notm}} settings for {{site.data.keyword.logs_routing_full_notm}}
{: #target-acct-reset-step1}
{: cli}
{: step}

To check your account's configuration and find out if there are default targets configured, run the following command:

```text
ibmcloud logs-router setting get
```
{: pre}

The setting **Permitted target regions** lists the locations in the account that have been configured as valid locations where targets can be created.

## How do I reset the default targets setting in {{site.data.keyword.logs_routing_full_notm}}
{: #target-acct-reset-step2}
{: cli}
{: step}

Run the following command to reset the account's default targets:

```sh
ibmcloud logs-router setting update --default-targets '[{}]'
```
{: pre}



## How do I reset permitted target regions in {{site.data.keyword.logs_routing_full}} using the UI
{: #reset_permitted_targets_ui}
{: ui}

You can remove all permitted target regions using the {{site.data.keyword.logs_routing_full_notm}} UI by updating your account settings and removing all permitted target regions. For more information, see [Configuring account settings](/docs/logs-router?topic=logs-router-settings&interface=ui).
