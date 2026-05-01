---

copyright:
  years:  2023, 2026
lastupdated: "2026-04-28"

keywords:

subcollection: logs-router

---

{{site.data.keyword.attribute-definition-list}}




# Configuring the CLI to manage {{site.data.keyword.logs_routing_full_notm}} account configuration
{: #logs-router-cli-config}

You must configure your local {{site.data.keyword.logs_routing_full_notm}} CLI so that you can manage the {{site.data.keyword.logs_routing_full_notm}} account configuration by using CLI commands.
{: shortdesc}

## Step 1: Installing the CLI locally
{: #logs-router-cli-config-install}


Complete the following steps to install the {{site.data.keyword.logs_routing_full_notm}} CLI:

1. [Install the {{site.data.keyword.cloud_notm}} CLI](/docs/cli?topic=cli-install-ibmcloud-cli).

2. Get the versions that are available for the `logs-router` CLI plug-in. Run the following command:

    ```sh
    ibmcloud plugin repo-plugins
    ```
    {: pre}

    For example, to get the versions of the {{site.data.keyword.logs_routing_full_notm}} CLI plug-in, run the following command:

    ```sh
    ibmcloud plugin repo-plugins | grep logs-router
    ```
    {: pre}

    The output of the command lists the versions that are available.

3. Install the CLI plugin in your local system. Run the following command:

    ```sh
    ibmcloud plugin install logs-router [-v VERSION]
    ```
    {: pre}

    Where `VERSION` is the value of a listed version that is available for the plug-in.


If you have the {{site.data.keyword.logs_routing_full_notm}} CLI plug-in in your local system and you want to update the version of the plug-in, run the following command: `ibmcloud plugin update logs-router`.
{: tip}

If you want to delete the {{site.data.keyword.logs_routing_full_notm}} CLI plug-in from your local system, run the following command: `ibmcloud plugin delete logs-router`.
{: tip}


## Step 2: Configuring the primary metadata region
{: #logs-router-cli-config-metadata}


Before you can use the {{site.data.keyword.logs_routing_full_notm}} CLI to manage routes or targets, you must configure a primary metadata region.
{: attention}

You can configure the primary metadata region by using the following command:

```sh
ibmcloud logs-router setting update command --primary-metadata-region PRIMARY-METADATA-REGION
```
{: codeblock}

Where `PRIMARY-METADATA-REGION` is set to a supported location. For more information, see [Locations](/docs/logs-router?topic=logs-router-locations).

## Updating the CLI
{: #logs-router-cli-config-update}

To update the CLI, run the following command:

```sh
ibmcloud plugin update logs-router
```
{: pre}

## Deleting the CLI
{: #logs-router-cli-config-delete}


To delete the CLI, run the following command:

```sh
ibmcloud plugin delete logs-router
```
{: pre}
