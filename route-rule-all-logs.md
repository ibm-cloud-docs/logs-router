---

copyright:
  years:  2023, 2026
lastupdated: "2026-04-28"

keywords:

subcollection: logs-router

---

{{site.data.keyword.attribute-definition-list}}


# Routing all platform logs
{: #route-rule-all-logs}

Route all platform logs that are generated in all of the {{site.data.keyword.logs_routing_full_notm}} supported locations to multiple destination targets.
{: shortdesc}

A rule consists of 1 or more targets, and 1 or more inclusion filters. For more information on how routes work, see [Understanding how routes work in your account](/docs/logs-router?topic=logs-router-routes&interface=cli#route_behaviour).

Inclusion filters determine which platform logs are routed to the targets.

To route all platform logs, you must exclude the `inclusion_filters` definition when you configure the route.
{: note}


## Prereqs
{: #route-rule-all-prereqs}
{: cli}

1. [Install the {{site.data.keyword.cloud_notm}} CLI](/docs/cli?topic=cli-install-ibmcloud-cli).

2. [Install the {{site.data.keyword.logs_routing_full_notm}} CLI](/docs/logs-router?topic=logs-router-logs-router-cli-config).

3. Ensure you have the [correct IAM permissions to configure {{site.data.keyword.logs_routing_full_notm}} routes.](/docs/logs-router?topic=logs-router-iam)

4. Log in to {{site.data.keyword.cloud_notm}}. Run the following command: [ibmcloud login](/docs/cli?topic=cli-ibmcloud_cli#ibmcloud_login).

## Get the target details
{: #route-rule-all-step1}
{: cli}
{: step}

Complete the following steps:

1. Run the following command to list all targets. Copy the target ID of the one where you want to route the platform logs.

    ```text
    ibmcloud logs-router target list
    ```
    {: pre}

    The output of the command looks as follows:

    ```text
    Targets
          created_at        2026-04-28T21:33:18.632Z
          destination_crn   crn:v1:bluemix:public:logs:au-syd:a/xxxxxxxxxx:619801ea-7cd8-4792-80cf-49a8359b4357::
          id                6f9137b3-2bb9-4724-851b-153e23c82d80
          managed_by        account
          name              cl-platform-logs-sydney-113
          region            eu-gb
          target_type       cloud_logs
          updated_at        2026-04-28T21:35:22.184Z
          write_status
                            status   success
    ```
    {: screen}

2. Run the following command to get the details of the targets where you want to route the platform logs:

    ```text
    ibmcloud logs-router target get --id <TARGET_ID>
    ```
    {: pre}

    The output of the command looks as follows:

    ```text
    Targets
          created_at        2026-04-28T22:10:59.021Z
          destination_crn   crn:v1:bluemix:public:logs:br-sao:a/xxxxxx:00f53f77-6b15-4f10-a432-b0ac46586073::
          id                0b4e6aa9-257c-4a3a-ae42-9a36a5b3adc7
          managed_by        account
          name              cl-target2
          region            eu-gb
          target_type       cloud_logs
          updated_at        2026-04-28T22:10:59.021Z
          write_status
                            status   success


          created_at        2026-04-28T21:33:18.632Z
          destination_crn   crn:v1:bluemix:public:logs:au-syd:a/xxxxxx:619801ea-7cd8-4792-80cf-49a8359b4357::
          id                6f9137b3-2bb9-4724-851b-153e23c82d80
          managed_by        account
          name              cl-platform-logs-sydney-113
          region            eu-gb
          target_type       cloud_logs
          updated_at        2026-04-28T21:35:22.184Z
          write_status
                            status   success

    ```
    {: screen}

## Configure the route
{: #route-rule-all-step2}
{: cli}
{: step}

Run the following command to send all platform logs from supported locations in your account to the targets identified in previous steps.

```text
ibmcloud logs-router route create --name route-all --rules '[{"action": "send", "targets":[{"id":"6f9137b3-2bb9-4724-851b-153e23c82d80"},{"id":"0b4e6aa9-257c-4a3a-ae42-9a36a5b3adc7"}]}]'
```
{: pre}

The output of the command looks as follows:

```text
Id           ea59a1b0-2e29-4a65-b785-0e07814942a7
Name         route-all
Rules
             action              send
             inclusion_filters   -
             targets
                                 id            6f9137b3-2bb9-4724-851b-153e23c82d80
                                 name          cl-platform-logs-sydney-113
                                 target_type   cloud-logs

                                 id            0b4e6aa9-257c-4a3a-ae42-9a36a5b3adc7
                                 name          cl-target2
                                 target_type   cloud-logs


Created At   2026-04-28T22:11:49.488Z
Updated At   2026-04-28T22:11:49.488Z
Managed By   account

```
{: screen}
