---

copyright:
  years: "2026"
lastupdated: "2026-05-04"

keywords:

subcollection: logs-router

---

{{site.data.keyword.attribute-definition-list}}

# Migration Guide: Transitioning from v1 to v3
{: #v3-migration}

This guide provides step-by-step instructions for migrating your {{site.data.keyword.logs_routing_full_notm}} service from version 1 (v1) to version 3 (v3). While v1 provides a regional concept, v3 offers a global approach with routes and filters to adjust routing of platform logs to your needs. The migration process involves configuring your new v3 environment and then switching from v1 to v3.
{: shortdesc}

Note that there will be a brief service interruption (approximately a few minutes) during the final migration step where platform logs are not received.
{: attention}

**WARNING**: This migration is irreversible. Once you have migrated to v3, you cannot move back to v1.
{: important}

## Common Migration Scenarios
{: #v3-migration-scenarios}

Before beginning your migration, it is important to understand which scenario best matches your current logging architecture. The following three scenarios represent the most common logging configurations and will help you determine the appropriate setup for your v3 environment.

### Scenario 1: Centralized Logging
{: #v3-migration-scenarios-1}

In a centralized logging configuration, all platform logs for your entire account are consolidated into a single {{site.data.keyword.logs_full_notm}} instance.

This approach simplifies log management by providing a unified view of all platform activities across your account.

To implement this scenario in v3, you will need to create one target pointing to your centralized {{site.data.keyword.logs_full_notm}} instance and [configure one route with a wildcard rule](/docs/logs-router?topic=logs-router-route-rule-all-logs) to capture all platform logs regardless of their source region.

### Scenario 2: Geographic Logging
{: #v3-migration-scenarios-2}

The geographic logging scenario is designed for organizations that maintain multiple logging instances distributed across different geographic locations.

In this configuration, platform logs from various regions are routed to their nearest or designated regional {{site.data.keyword.logs_full_notm}} instance based on geographic proximity or data residency requirements. This approach balances centralized visibility with geographic distribution, allowing you to route logs from multiple regions to a smaller number of strategically located {{site.data.keyword.logs_full_notm}} instances.

To implement this scenario, you will need to create multiple targets (one for each geographic {{site.data.keyword.logs_full_notm}} instance) and configure routes with appropriate region-based filters to direct logs to the correct geographic destination.

### Scenario 3: Regional Logging
{: #v3-migration-scenarios-3}

Regional logging represents the most distributed approach, where each {{site.data.keyword.cloud_notm}} region has its own dedicated {{site.data.keyword.logs_full_notm}} instance.

This configuration ensures complete regional isolation of log data and is often used to meet strict data residency or compliance requirements. In this scenario, platform logs generated in a specific region are routed exclusively to the {{site.data.keyword.logs_full_notm}} instance deployed in that same region.

To implement this scenario in v3, you will need to create a separate target and route for each region where you operate, ensuring that each regional route includes filters that restrict log routing to only that region's {{site.data.keyword.logs_full_notm}} instance.

## Migration Approaches
{: #v3-migration-approaches}

There are two approaches to migrate from v1 to v3. Choose the approach that best fits your needs.

1. **Automated Migration (Recommended)**:

    The automated migration approach is recommended for most users as it simplifies the migration process and reduces the risk of configuration errors.
    {: note}

    Use the migration APIs to automatically generate v3 targets and routes based on your existing v1 tenants.

    For more information, see [Automated Migration](/docs/logs-router?topic=logs-router-v3-migration-automated).

2. **Manual Configuration**:

    Manually configure your v3 environment from scratch before completing the migration.

    Choose one of the following options:

    - [Manual migration from V1 to V3 for new accounts (No V1 Setup)](/docs/logs-router?topic=logs-router-v3-migration-manual-existing).

    - [Manual migration from v1 to v3 for existing accounts (With V1 Setup)](/docs/logs-router?topic=logs-router-v3-migration-manual-new-acc).


## Understanding Migration States
{: #v3-migration-states}

The migration process progresses through several states:

- **BEFORE**: The initial state before migration has started. If a previous migration attempt failed, the error message will be included.
- **IN_PROGRESS**: The migration process is actively running. This may take several minutes depending on your configuration.
- **PENDING_COMPLETION**: Your v3 routes and targets have been created successfully. You should verify the configuration before completing the migration.
- **COMPLETE**: The migration has been completed successfully, and your account is now using the v3 configuration.


## IAM Permissions for Migration
{: #v3-migration-iam}

The migration process requires specific IAM permissions. The following global actions are available for managing the migration:

- **logs-router.migration.post**: Configures and starts the migration. This is a two-step process to set up account metadata and then start the actual update of all regions. Requires Administrator role.
- **logs-router.migration.get**: Retrieves the status of the migration. Available to Administrator, Editor, Operator, and Viewer roles.
- **logs-router.migration.delete**: Deletes the generated migration plan, including all automatically created v3 targets and routes. Requires Administrator role.

You must have the **administrator** platform role to migrate from V1 to V3.
{: important}

For more information about IAM roles and permissions, see [IAM roles](/docs/logs-router?topic=logs-router-iam#iam-bytask).
