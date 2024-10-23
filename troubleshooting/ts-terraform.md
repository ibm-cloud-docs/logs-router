---

copyright:
  years:  2023, 2024
lastupdated: "2024-10-23"

keywords:

subcollection: logs-router

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why does Terraform show the error "CreateTenantWithContext failed: Conflict"?
{: #ts-terraform-conflict}
{: troubleshoot}
{: support}

An account can only create a single {{site.data.keyword.logs_routing_full_notm}} tenant per region with {{site.data.keyword.logs_full_notm}} or {{site.data.keyword.la_full_notm}} targets. This error indicates there must be an existing tenant in the region specified, or a target of either type already exists in a tenant.
{: shortdesc}

Trying to create a tenant with Terraform in a region where there is already an existing tenant not managed by Terraform will result in the Terraform error `CreateTenantWithContext failed: Conflict`. This error can also occur when trying to add a target of type {{site.data.keyword.logs_full_notm}} or {{site.data.keyword.la_full_notm}} to a tenant that already has that target type.
{: tsSymptoms}

This error occurs when attempting to create a tenant in a region where a tenant already exists, or attempting to add a target in a tenant that already contains that target type.
{: tsCauses}

Use the {{site.data.keyword.logs_routing_full_notm}} [API](/docs/logs-router?topic=logs-router-tenant-get) or the [console](/docs/logs-router?topic=logs-router-tenants-list#tenants-list-ui) to check if a tenant exists in a particular region and remove it if necessary. 
{: tsResolve}

Check the property `log_sink_crn` in the Terraform definition. There can be at most one target with a `log_sink_crn` of either type: {{site.data.keyword.logs_full_notm}} or {{site.data.keyword.la_full_notm}}.
