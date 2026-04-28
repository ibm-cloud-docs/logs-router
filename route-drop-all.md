---

copyright:
  years:  2023, 2026
lastupdated: "2026-04-28"

keywords:

subcollection: logs-router

---

{{site.data.keyword.attribute-definition-list}}


# Excluding all metrics by using the drop action
{: #route-drop-all}

You can configure {{site.data.keyword.logs_routing_full_notm}} to exclude (drop) metrics based on a configured rule. Dropped metrics are not sent on to a target.
{: shortdesc}

## Prereqs
{: #route-drop-all-prereqs}
{: cli}

1. [Install the {{site.data.keyword.cloud_notm}} CLI](/docs/cli?topic=cli-install-ibmcloud-cli).

2. [Install the {{site.data.keyword.logs_routing_full_notm}} CLI](/docs/metrics-router?topic=metrics-router-metrics-router-cli-config).

3. Ensure you have the [correct IAM permissions to configure {{site.data.keyword.logs_routing_full_notm}} routes.](/docs/metrics-router?topic=metrics-router-iam)

4. Log in to {{site.data.keyword.cloud_notm}}. Run the following command: [ibmcloud login](/docs/cli?topic=cli-ibmcloud_cli#ibmcloud_login).



## Configure the route
{: #route-drop-all-step2}
{: cli}

Run the following command to exclude all metrics received by {{site.data.keyword.logs_routing_full_notm}} across all regions in the account.

```text
ibmcloud metrics-router route create --name drop-all-route --rules '[{"action":"drop"}]'
```
{: pre}

The output of the command looks as follows:

```text
Your request includes a drop rule. Do you want to continue? [y/N]> y
OK
Route
Name:         drop-all-route
ID:           4966172b-75c9-4adf-9cbb-36562a421b5b
CRN:          crn:v1:bluemix:public:metrics-router:global:a/xxxxxxxx::route:4966172b-75c9-4adf-9cbb-36562a421b5b
Rule 0:       [drop, []]
Created At:   2023-05-30T14:52:02.148Z
Updated At:   2023-05-30T14:52:02.148Z
```
{: screen}
