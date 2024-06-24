---

copyright:
  years:  2022, 2023
lastupdated: "2023-12-29"

keywords:

subcollection: logs-router

---

{{site.data.keyword.attribute-definition-list}}

# Setting up IAM permissions for ingestion
{: #agent-iam-permissions}

To send logs to {{site.data.keyword.logs_routing_full_notm}}, you must have the `Writer` role for {{site.data.keyword.logs_routing_full_notm}}.
{: shortdesc}

To see what IAM roles are available for {{site.data.keyword.logs_routing_full_notm}}, see [managing IAM access](/docs/logs-router?topic=logs-router-iam).

## Setting up permissions for ingestion by using the CLI
{: #agent-iam-permissions-cli}
{: cli}

Granting the role can be done by using the `ibmcloud` CLI.

Use the appropriate command for the type of identity:

| Type of identity  | Command |
|-------------------|---------|
| User account      | `ibmcloud iam user-policy-create <username> --roles Writer --service-name logs-router` |
| Service ID        | `ibmcloud iam service-policy-create <serviceID> --roles Writer --service-name logs-router` |
| Trusted profile   | `ibmcloud iam tp-policy-create <trustedProfile> --roles Writer --service-name logs-router` |
{: caption="Table 1. Command to grant IAM permissions by type of identity" caption-side="top"}

Instead of assigning roles directly to identities, a common strategy is to assign roles to access groups, and add identities as members to those access groups. For more information about access groups, see [setting up access groups.](/docs/account?topic=account-groups&interface=cli)
{: tip}
