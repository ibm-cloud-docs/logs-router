---

copyright:
  years:  2023, 2024
lastupdated: "2024-07-30"

keywords:

subcollection: logs-router

---

{{site.data.keyword.attribute-definition-list}}

# Setting up IAM permissions for managing tenants
{: #tenant-iam-permissions}

To manage tenants for an account, you must have the `Manager` role for {{site.data.keyword.logs_routing_full_notm}}. To see what IAM roles are available for {{site.data.keyword.logs_routing_full_notm}}, see [managing IAM access](/docs/logs-router?topic=logs-router-iam).
{: shortdesc}

If you are the account owner, you might already have sufficient access without requiring additional permissions.
{: tip}

## Setting up permissions for managing tenants by using the CLI
{: #tenant-iam-permissions-cli}
{: cli}

Granting the role can be done by using the `ibmcloud` CLI.

Use the appropriate command for the type of identity:

| Type of identity  | Command |
|-------------------|---------|
| User account      | `ibmcloud iam user-policy-create <username> --roles Manager --service-name logs-router` |
| Service ID        | `ibmcloud iam service-policy-create <serviceID> --roles Manager --service-name logs-router` |
| Trusted profile   | `ibmcloud iam tp-policy-create <trustedProfile> --roles Manager --service-name logs-router` |
| Access group      | `ibmcloud iam access-group-policy-create ACCESS_GROUP --roles Manager --service-name logs-router` |
{: caption="Table 1. Command to grant IAM permissions by type of identity" caption-side="top"}

Instead of assigning roles directly to identities, a common strategy is to assign roles to access groups, and add identities as members to those access groups. For more information about access groups, see [setting up access groups.](/docs/account?topic=account-groups&interface=cli)
{: tip}
