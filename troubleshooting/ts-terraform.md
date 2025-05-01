---

copyright:
  years:  2023, 2025
lastupdated: "2025-04-30"

keywords:

subcollection: logs-router

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why does Terraform show the error "CreateTenantWithContext failed: Conflict"?
{: #ts-terraform-conflict}
{: troubleshoot}
{: support}

An account can only create a single {{site.data.keyword.logs_routing_full_notm}} tenant per region with a single target. This error indicates there must be an existing tenant in the region specified, or a target already exists in a tenant.
{: shortdesc}

Trying to create a tenant with Terraform in a region where there is already an existing tenant not managed by Terraform will result in the Terraform error `CreateTenantWithContext failed: Conflict`. This error can also occur when trying to add a target when one already exists. 
{: tsSymptoms}

This error occurs when attempting to create a tenant in a region where a tenant already exists, or attempting to add a target in a tenant that already contains that target type.
{: tsCauses}

Use the {{site.data.keyword.logs_routing_full_notm}} [API](/docs/logs-router?topic=logs-router-tenant-get) or the [console](/docs/logs-router?topic=logs-router-tenants-list#tenants-list-ui) to check if a tenant exists in a particular region and remove it if necessary. 
{: tsResolve}

Check the property `log_sink_crn` in the Terraform definition. There can be at most one target with a `log_sink_crn`.
