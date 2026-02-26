---

copyright:
  years:  2026, 2026
lastupdated: "2026-02-26"

keywords: enterprise

subcollection: logs-router

---

{{site.data.keyword.attribute-definition-list}}

# Enterprise-managed routing
{: #enterprise-routing-scenario}


Use {{site.data.keyword.logs_routing_full}} to configure the routing of platform logs from your enterprise child accounts to your enterprise parent account, then use {{site.data.keyword.iamshort}} (IAM) Action Control to restrict changes to the enterprise-managed routing.
{: shortdesc}

You can configure your enterprise child accounts to route all or a subset of platform logs to an enterprise parent account. The child accounts contain Cloud resources that generate platform logs. The parent account owns the enterprise and has {{site.data.keyword.logs_full_notm}} instances that will contain the platform logs from the child accounts.

The enterprise-managed routing configurations, from child accounts to parent account, can be locked by restricting the {{site.data.keyword.logs_routing_full}} enterprise actions with IAM Action Control. The child accounts can continue to manage their own account-managed {{site.data.keyword.logs_routing_full}} configurations, while the enterprise-managed configurations are restricted.

This example uses terraform automation to facilitate consistent routing configurations and Action Control restrictions across many child accounts. However, other configuration methods can be used as well.
{: tip}

![A diagram that shows a sample {{site.data.keyword.logs_routing_full}} enterprise configuration.](/images/logs-router-enterprise-routing.svg "{{site.data.keyword.logs_routing_full}} enterprise configuration."){: caption="{{site.data.keyword.logs_routing_full}} enterprise configuration" caption-side="bottom"}

## Before you begin
{: #enterprise-routing-before-begin}

- This document only supports version 3 of the {{site.data.keyword.logs_routing_full}} API, see [Transitioning from v1 to v3](/docs/logs-router?topic=v3-migration) to migrate your child accounts.

- To learn about how enterprise-manged IAM templates make your enterprise more secure, see [How enterprise-managed IAM access works](/docs/enterprise-management?topic=enterprise-management-access-enterprises&interface=ui#how-enterprise-iam).

- Be a member of the enterprise account to create and assign enterprise-managed IAM templates.

- To create, update, and delete an enterprise-managed IAM template, make sure that you are assigned with the following access:
   * A policy with the Template Administrator role on All IAM Account Management services.

- To assign an enterprise-managed IAM template to child accounts, make sure that you are assigned with the following access:
   * A policy with the Template Assignment Administrator role on All IAM Account Management services
   * A policy with at least the Viewer role on the Enterprise service.

- To create and assign action control templates, new and existing accounts in your enterprise must opt-in to enterprise-managed IAM. For more information, see [Opting in to enterprise-managed IAM](/docs/enterprise-management?topic=enterprise-management-enterprise-managed-opt-in).

- You must have Administrator role with permissions to manage {{site.data.keyword.logs_routing_full_notm}} in the source accounts. See [Managing access with IAM](/docs/logs-router?topic=logs-router-iam) and [Granting IAM permissions](/docs/logs-router?topic=logs-router-iam-permissions).

## Step 1 - Create the parent account destination instance
{: #step-create-target-instance-terraform}
{: terraform}

Follow [Provisioning an {{site.data.keyword.logs_full_notm}} instance by using Terraform](/docs/cloud-logs?topic=cloud-logs-terraform-setup) to create the instance in the enterprise parent account. The instance will not receive platform logs until the child accounts are configured to send their logs to the instance.

## Step 2 - Create parent authorization policies
{: #step-create-authorization-policy-terraform}
{: terraform}

Create an {{site.data.keyword.iamshort}} (IAM) service to service authorization policy in the parent account for each of your child accounts. In this example there are two child accounts.

```terraform
# FILE: parent/main.tf

terraform {
  required_version = ">= 0.13"
  required_providers {
    ibm = {
      source  = "ibm-cloud/ibm"
      version = "~>1.88.3"
    }
  }
}

variable "ibmcloud_api_key" {
  description = "The IBM Cloud API key."
  type        = string
  sensitive   = true
}

provider "ibm" {
  ibmcloud_api_key = var.ibmcloud_api_key
}

locals {
  child_accounts = [
    "<INPUT_CHILD_A_ACCOUNT_ID>",
    "<INPUT_CHILD_B_ACCOUNT_ID>"
  ]
}

resource "ibm_iam_authorization_policy" "cross_account_policy" {
  for_each = toset(local.child_accounts)
  source_service_name         = "logs-router"
  source_service_account      = each.key
  target_service_name         = "logs"
  target_resource_instance_id = "<INPUT_PARENT_LOGS_INSTANCE_ID>"
  roles                       = ["Sender"]
  description                 = "Authorize enterprise child account routing to the parent"
}
```
{: codeblock}

This step ensures {{site.data.keyword.logs_routing_full}} is authorized to route child A and child B account platform logs to parent account.


## Step 3 - Create enterprise targets in child accounts
{: #step-create-child-enterprise-targets}
{: terraform}

Next, you need to create {{site.data.keyword.logs_routing_full}} enterprise targets in each child account, the target destination is the parent account's {{site.data.keyword.logs_full_notm}} instance. In this example, there are only two child accounts, but there is no limitation to the number of child accounts that can be targeted to a single parent account.

Note the `managed_by = "enterprise"` parameter below.

```terraform
# NEW FILE: child_a/main.tf

terraform {
  required_version = ">= 0.13"
  required_providers {
    ibm = {
      source  = "ibm-cloud/ibm"
      version = "~>1.88.3"
    }
  }
}

variable "ibmcloud_api_key" {
  description = "The IBM Cloud API key."
  type        = string
  sensitive   = true
}

provider "ibm" {
  ibmcloud_api_key = var.ibmcloud_api_key
}

resource "ibm_logs_router_settings" "logs_router_settings" {
  primary_metadata_region = "<INPUT_LOGS_ROUTING_REGION>"
}

resource "ibm_logs_router_target" "logs_router_target" {
  destination_crn = "<INPUT_PARENT_CLOUD_LOGS_CRN>"
  name            = "child-a-target"
  managed_by      = "enterprise"
}
```
{: codeblock}

```terraform
# NEW FILE: child_b/main.tf

terraform {
  required_version = ">= 0.13"
  required_providers {
    ibm = {
      source  = "ibm-cloud/ibm"
      version = "~>1.81.0"
    }
  }
}

variable "ibmcloud_api_key" {
  description = "The IBM Cloud API key."
  type        = string
  sensitive   = true
}

provider "ibm" {
  ibmcloud_api_key = var.ibmcloud_api_key
}

resource "ibm_logs_router_settings" "logs_router_settings" {
  primary_metadata_region = "<INPUT_LOGS_ROUTING_REGION>"
}

resource "ibm_logs_router_target" "logs_router_target" {
  destination_crn = "<INPUT_PARENT_CLOUD_LOGS_CRN>"
  name            = "child-b-target"
  managed_by      = "enterprise"
}
```
{: codeblock}

This step creates {{site.data.keyword.logs_routing_full}} enterprise targets pointing to the parent {{site.data.keyword.logs_full_notm}} instance. Routing is not yet enabled.

## Step 4 - Create enterprise routes in the child accounts
{: #step-create-child-enterprise-routes}
{: terraform}

Create {{site.data.keyword.logs_routing_full}} enterprise routes in each child account. You can route all platform logs to the parent or a subset of platform logs to the parent. In this example, child A and child B accounts will route all platform logs to the parent.

See [IBM Cloud services that generate platform logs](/docs/logs-router?topic=logs-router-cloud_services).

```terraform
# FILE: child_a/main.tf

# Previous steps...

resource "ibm_logs_router_route" "logs_router_route" {
  lifecycle {
    create_before_destroy = true
  }
  name       = "child-a-route"
  managed_by = "enterprise"

  # Route all platform logs to the hub.
  rules {
    action = "send"
    targets {
      id = ibm_logs_router_target.logs_router_target.id
    }
  }
}
```
{: codeblock}

```terraform
# FILE: child_b/main.tf

# Previous steps...

resource "ibm_logs_router_route" "logs_router_route" {
  lifecycle {
    create_before_destroy = true
  }
  name       = "child-b-route"
  managed_by = "enterprise"

  # Route all platform logs to the hub.
  rules {
    action = "send"
    targets {
      id = ibm_logs_router_target.logs_router_target.id
    }
  }
}
```
{: codeblock}

This step creates the {{site.data.keyword.logs_routing_full}} enterprise routes. Platform logs will be routed to the parent {{site.data.keyword.logs_full_notm}} instance.


## Step 5 - Restrict enterprise actions with IAM Action Control
{: #step-restrict-enterprise-actions}
{: terraform}

Now that the enterprise-managed routing is created, you must restrict the usage of {{site.data.keyword.logs_routing_full}} enterprise actions so that the enterprise-managed routing cannot be modified by any entity. In this example, there is only one version of the action control template and it is assigned to each individual Account instead of the entire Enterprise or an Account Group.

In order to make changes to the enterprise-managed routing, you must delete the action control assignments.
{: note}

```terraform
# FILE: parent/main.tf

# Previous steps...

resource "ibm_iam_action_control_template" "logs_router_enterprise_action_control_template" {
  name = "Logs Router Enterprise Routing"
  description = "Restrict Logs Router enterprise-managed routing"
  action_control {
    actions = [
      "logs-router.enterprise-target.create",
      "logs-router.enterprise-target.update",
      "logs-router.enterprise-target.delete",
      "logs-router.enterprise-route.create",
      "logs-router.enterprise-route.update",
      "logs-router.enterprise-route.delete"
    ]
    service_name = "logs-router"
  }
  committed = "true"
}

locals {
  child_accounts = [
    "<INPUT_CHILD_A_ACCOUNT_ID>",
    "<INPUT_CHILD_B_ACCOUNT_ID>"
  ]
}

resource "ibm_iam_action_control_assignment" "logs_router_enterprise_action_control_assignment" {
  for_each = toset(local.child_accounts)
  target = {
    type = "Account"
    id   = each.key
  }
  templates {
    id      = ibm_iam_action_control_template.logs_router_enterprise_action_control_template.template_id
    version = ibm_iam_action_control_template.logs_router_enterprise_action_control_template.version
  }
}
```
{: codeblock}

This step ensures {{site.data.keyword.logs_routing_full}} enterprise actions are restricted from further use, locking the enterprise-managed routing.


## Optional Step 6 - Account-managed routing
{: #step-account-managed-routing}
{: terraform}

Optionally, you can create account-managed routing in the enterprise child accounts or parent account; it will not conflict with the enterprise-managed routing created earlier. To create account-managed routing, ensure that the {{site.data.keyword.logs_routing_full}} target and route configurations have `managed_by = "account"`.
