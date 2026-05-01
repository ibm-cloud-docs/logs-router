---

copyright:
  years:  2023, 2026
lastupdated: "2026-04-28"

keywords:

subcollection: logs-router

---

{{site.data.keyword.attribute-definition-list}}


# Enforcing target locations
{: #target-locations}

To enforce target locations in your {{site.data.keyword.cloud_notm}} account, you must configure {{site.data.keyword.logs_routing_full_notm}} account settings to limit the locations.
{: shortdesc}



## Configuring permitted target regions using the UI
{: #permitted_targets_ui}
{: ui}

You can configure permitted target regions using the {{site.data.keyword.logs_routing_full_notm}} UI. For more information, see [Configuring account settings](/docs/logs-router?topic=logs-router-settings&interface=ui).



## Prereqs
{: #target-locations-prereqs}
{: cli}

1. [Install the {{site.data.keyword.cloud_notm}} CLI](/docs/cli?topic=cli-install-ibmcloud-cli).

2. [Install the {{site.data.keyword.logs_routing_full_notm}} CLI](/docs/logs-router?topic=logs-router-logs-router-cli-config).

3. Ensure you have the [correct IAM permissions to configure {{site.data.keyword.logs_routing_full_notm}} settings](/docs/logs-router?topic=logs-router-iam).

4. Log in to {{site.data.keyword.cloud_notm}} by running the following command: [ibmcloud login](/docs/cli?topic=cli-ibmcloud_cli#ibmcloud_login)



## Step 1. Get details of your current account settings
{: #target-locations-step1}
{: cli}

Get details of your current account settings. Run the following command:

```text
ibmcloud logs-router setting get
```
{: pre}

The setting **Permitted target regions** list all the locations where you allowed administrators to configure targets in the account.

## Step 2.
{: #target-locations-step2}
{: cli}

To manage the list of permitted regions, run the following command:

```text
ibmcloud logs-router setting update --permitted-target-regions REGIONS
```
{: pre}

Where `REGIONS` define a comma-separated list of regions.

For example, you can run the following command to set 3 regions only:

```text
ibmcloud logs-router setting update --permitted-target-regions "us-south","us-east","eu-de"
```
{: screen}
