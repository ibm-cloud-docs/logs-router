---

copyright:
  years: "2026"
lastupdated: "2026-04-29"

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

    Use the migration APIs to automatically generate v3 targets and routes based on your existing v1 tenants. This approach is covered in the Automated Migration section. For more information, see [Automated Migration](/docs/logs-router?topic=logs-router-v3-migration&interface=cli#v3-migration-automated).

2. **Manual Configuration**:

    Manually configure your v3 environment from scratch before completing the migration. This approach is covered in the Manual Migration section. For more information, see [Manual Migration](docs/logs-router?topic=logs-router-v3-migration&interface=cli#v3-migration-manual).


## Understanding Migration States
{: #v3-migration-states}

The migration process progresses through several states:

- **BEFORE**: The initial state before migration has started. If a previous migration attempt failed, the error message will be included.
- **IN_PROGRESS**: The migration process is actively running. This may take several minutes depending on your configuration.
- **PENDING_COMPLETION**: Your v3 routes and targets have been created successfully. You should verify the configuration before completing the migration.
- **COMPLETE**: The migration has been completed successfully, and your account is now using the v3 configuration.

## Automated Migration
{: #v3-migration-automated}

The automated migration process uses the migration APIs to generate v3 targets and routes based on your existing v1 tenant configuration.

This approach streamlines the migration by automatically creating the necessary v3 resources.

### Step 1: Generate the Migration Plan
{: #v3-migration-generate}

The first step is to generate a migration plan. This is an asynchronous process that analyzes your existing v1 tenants and creates corresponding v3 targets and routes.

Run the following API call to generate the migration plan:

```sh
curl -X POST \
  "https://api.<region>.logs-router.cloud.ibm.com/v3/migrate?action=generate" \
  -H "Authorization: Bearer <IAM_TOKEN>"
```
{: codeblock}

Where `<region>` is your primary metadata region.

The authorization bearer token must have the `logs-router.migration.post` permission (Administrator role) for the {{site.data.keyword.logs_routing_full_notm}} service. For more information, see [IAM roles](/docs/logs-router?topic=logs-router-iam#iam-bytask).

### Step 2: Check Migration Status
{: #v3-migration-status}

After initiating the migration plan generation, you need to monitor its progress. Use the following API call to check the status:

```sh
curl -X GET \
  "https://api.<region>.logs-router.cloud.ibm.com/v3/migrate" \
  -H "Authorization: Bearer <IAM_TOKEN>"
```
{: codeblock}

The authorization bearer token must have the `logs-router.migration.get` permission (Administrator, Editor, Operator, or Viewer role).

The API will return one of the following responses based on the current state:

**COMPLETE state** (HTTP 200 OK):
```json
{
  "version": "3",
  "state": "COMPLETE",
  "message": "Migration complete"
}
```
{: codeblock}

**BEFORE state** (HTTP 200 OK):
```json
{
  "version": "1",
  "state": "BEFORE",
  "message": "Error message with reason for previous failure"
}
```
{: codeblock}

If the message is empty, no previous migration attempt has been made or failed.

**IN_PROGRESS state** (HTTP 200 OK):
```json
{
  "version": "1",
  "state": "IN_PROGRESS",
  "message": "The migration process is still running - check back in a few minutes"
}
```
{: codeblock}

**PENDING_COMPLETION state** (HTTP 200 OK):
```json
{
  "version": "1",
  "state": "PENDING_COMPLETION",
  "message": "Your v3 routes and targets have been created - verify the content and then use action=complete to switch your account permanently to the v3 configuration."
}
```
{: codeblock}

Continue checking the status until the state changes to `PENDING_COMPLETION`.

### Step 3: Review Generated Targets and Routes
{: #v3-migration-review}

Once the migration reaches the `PENDING_COMPLETION` state, you should review the automatically generated v3 targets and routes to ensure they match your requirements.

To list all targets, run:

```sh
curl -X GET \
  "https://api.<region>.logs-router.cloud.ibm.com/v3/targets" \
  -H "Authorization: Bearer <IAM_TOKEN>"
```
{: codeblock}

To list all routes, run:

```sh
curl -X GET \
  "https://api.<region>.logs-router.cloud.ibm.com/v3/routes" \
  -H "Authorization: Bearer <IAM_TOKEN>"
```
{: codeblock}

For more information about these API endpoints, see [List targets](/apidocs/logs-router-service-api/logs-router-v3#list-targets){: external} and [List routes](/apidocs/logs-router-service-api/logs-router-v3#list-routes){: external}.

### Step 4: Modify Configuration (Optional)
{: #v3-migration-modify}

If you need to make changes to the generated targets or routes, you can update them using the v3 API endpoints before completing the migration.

To update a target:

```sh
curl -X PATCH \
  "https://api.<region>.logs-router.cloud.ibm.com/v3/targets/<target_id>" \
  -H "Authorization: Bearer <IAM_TOKEN>" \
  -H 'content-type: application/json' \
  -d '{
    "name": "<NEW_NAME>"
  }'
```
{: codeblock}

To update a route:

```sh
curl -X PATCH \
  "https://api.<region>.logs-router.cloud.ibm.com/v3/routes/<route_id>" \
  -H "Authorization: Bearer <IAM_TOKEN>" \
  -H 'content-type: application/json' \
  -d '{
    "name": "<NEW_NAME>",
    "rules": [...]
  }'
```
{: codeblock}

For detailed information about updating targets and routes, see [Update target](/apidocs/logs-router-service-api/logs-router-v3#update-target){: external} and [Update route](/apidocs/logs-router-service-api/logs-router-v3#update-route){: external}.

### Step 5: Delete Migration Plan (Optional)
{: #v3-migration-delete}

If you need to start over or cancel the migration, you can delete the generated migration plan. This will remove all automatically created v3 targets and routes.

Run the following API call to delete the migration plan:

```sh
curl -X DELETE \
  "https://api.<region>.logs-router.cloud.ibm.com/v3/migrate" \
  -H "Authorization: Bearer <IAM_TOKEN>"
```
{: codeblock}

The authorization bearer token must have the `logs-router.migration.delete` permission (Administrator role).

After deleting the migration plan, you can start the process again from Step 1.

### Step 6: Complete the Migration
{: #v3-migration-complete}

Once you have reviewed and are satisfied with the generated v3 configuration, you can complete the migration. This action is irreversible and will switch your account from v1 to v3.

Run the following API call to complete the migration:

```sh
curl -X POST \
  "https://api.<region>.logs-router.cloud.ibm.com/v3/migrate?action=complete" \
  -H "Authorization: Bearer <IAM_TOKEN>"
```
{: codeblock}

The authorization bearer token must have the `logs-router.migration.post` permission (Administrator role).

After executing this command:

1. The v1 service will shut down and stop processing logs.
## Manual Migration
{: #v3-migration-manual}

If you prefer to manually configure your v3 environment, the process differs depending on whether you have an existing v1 setup or are starting fresh with a new account.

### For New Accounts (No V1 Setup)
{: #v3-migration-manual-new}

If you are setting up {{site.data.keyword.logs_routing_full_notm}} for the first time without any existing v1 configuration, follow these steps:


#### Step 1: Set Primary Region and Version
{: #v3-migration-manual-new-step-1}

Configure the primary metadata region and set the API version to v3 in a single operation:

```sh
curl -X PATCH \
  "https://api.<region>.logs-router.cloud.ibm.com/v3/settings" \
  -H "Authorization: Bearer <IAM_TOKEN>" \
  -H 'content-type: application/json' \
  -d '{
    "primary_metadata_region": "<region>",
    "api_version": 3
  }'
```
{: codeblock}

Where `<region>` is your desired region such as `us-south`, or `eu-de`.

### Step 1.2: Generate a V3 configuration from your existing V1 configuration
{: #v3-migration-s1-configure-v3-generate}

The current migration process requires that you begin the migration by generating a configuration from the existing V1 setup.  This step will create routes and targets based on the V1 targets.

```sh
curl -X POST \
  https://api.<region>.logs-router.cloud.ibm.com/v3/migrate?action=generate \
  -H "Authorization: Bearer <IAM_TOKEN>" \
  -H 'content-type: application/json'
```

 Once the routes and targets are generated, you can review the generated configurations and either proceed with steps 1.3 and 1.4 or move ahead to Section 2 to complete the migration.  Refer to the [{{site.data.keyword.logs_routing_full_notm}} v3 API documentation](/apidocs/logs-router-service-api/logs-router-v3){: external} for more details.

### Step 1.3: Create a Target for Platform Logs
{: #v3-migration-s1-configure-v3-create-target}


#### Step 2: Create Targets
{: #v3-migration-manual-new-step-2}

Create target destinations where your platform logs will be delivered:

```sh
curl -X POST \
  "https://api.<region>.logs-router.cloud.ibm.com/v3/targets" \
  -H "Authorization: Bearer <IAM_TOKEN>" \
  -H 'content-type: application/json' \
  -d '{
    "name": "<NAME>",
    "destination_crn": "<CRN>"
  }'
```
{: codeblock}

Where:
- `<NAME>` is a descriptive name for your target
- `<CRN>` is the Cloud Resource Name of your destination {{site.data.keyword.logs_full_notm}} instance

Save the target ID from the response for use in the next step.

For more information, see [Create target](/apidocs/logs-router-service-api/logs-router-v3#create-target){: external}


#### Step 3: Create Routes
{: #v3-migration-manual-new-step-3}

### Step 1.4: Create a Route to the Target
{: #v3-migration-s1-configure-v3-create-route}


Create routes to define how logs are directed to your targets. Logs will start flowing once the route is defined:

```sh
curl -X POST \
  "https://api.<region>.logs-router.cloud.ibm.com/v3/routes" \
  -H "Authorization: Bearer <IAM_TOKEN>" \
  -H 'content-type: application/json' \
  -d '{
    "name": "<NAME>",
    "rules": [
      {
        "action": "send",
        "targets": [
          {
            "id": "<TARGET_ID>"
          }
        ]
      }
    ]
  }'
```
{: codeblock}

Where:
- `<NAME>` is a descriptive name for your route
- `<TARGET_ID>` is the target ID from the previous step

For detailed information about route configuration options, including filtering capabilities, see [Create route](/apidocs/logs-router-service-api/logs-router-v3#create-route){: external}

### For Existing Accounts (With V1 Setup)
{: #v3-migration-manual-existing}

If you have an existing v1 configuration and want to manually migrate to v3, follow these steps:

#### Step 1: Set Primary Metadata Region
{: #v3-migration-manual-existing-step-1}

Configure the primary metadata region for your v3 setup:

```sh
curl -X PATCH \
  "https://api.<region>.logs-router.cloud.ibm.com/v3/settings" \
  -H "Authorization: Bearer <IAM_TOKEN>" \
  -H 'content-type: application/json' \
  -d '{
    "primary_metadata_region": "<region>"
  }'
```
{: codeblock}

Where `<region>` is your desired region such as `us-south`, or `eu-de`.

For detailed information about this API endpoint, see [Modify settings](/apidocs/logs-router-service-api/logs-router-v3#update-settings){: external}

#### Step 2: Create Targets
{: #v3-migration-manual-existing-step-2}

Create target destinations for your v3 configuration:

```sh
curl -X POST \
  "https://api.<region>.logs-router.cloud.ibm.com/v3/targets" \
  -H "Authorization: Bearer <IAM_TOKEN>" \
  -H 'content-type: application/json' \
  -d '{
    "name": "<NAME>",
    "destination_crn": "<CRN>"
  }'
```
{: codeblock}

Where:
- `<NAME>` is a descriptive name for your target
- `<CRN>` is the Cloud Resource Name of your destination {{site.data.keyword.logs_full_notm}} instance

Save the target ID from the response for use in the next step. Repeat this step for each target you need based on your migration scenario.

For more information, see [Create target](/apidocs/logs-router-service-api/logs-router-v3#create-target){: external}

#### Step 3: Create Routes
{: #v3-migration-manual-existing-step-3}

Create routes to define how logs should be directed to your targets:

```sh
curl -X POST \
  "https://api.<region>.logs-router.cloud.ibm.com/v3/routes" \
  -H "Authorization: Bearer <IAM_TOKEN>" \
  -H 'content-type: application/json' \
  -d '{
    "name": "<NAME>",
    "rules": [
      {
        "action": "send",
        "targets": [
          {
            "id": "<TARGET_ID>"
          }
        ]
      }
    ]
  }'
```
{: codeblock}

Where:
- `<NAME>` is a descriptive name for your route
- `<TARGET_ID>` is the target ID from the previous step

You can add filters to customize log routing. For example, to route logs from specific regions:

```sh
curl -X POST \
  "https://api.<region>.logs-router.cloud.ibm.com/v3/routes" \
  -H "Authorization: Bearer <IAM_TOKEN>" \
  -H 'content-type: application/json' \
  -d '{
    "name": "<NAME>",
    "rules": [
      {
        "action": "send",
        "targets": [
          {
            "id": "<TARGET_ID>"
          }
        ],
        "inclusion_filters": [
          {
            "operand": "location",
            "operator": "in",
            "values": ["us-south", "us-east"]
          }
        ]
      }
    ]
  }'
```
{: codeblock}

For detailed information about route configuration options, see [Create route](/apidocs/logs-router-service-api/logs-router-v3#create-route){: external}

#### Step 4: Switch to V3
{: #v3-migration-manual-existing-step-4}

Once you have configured all your v3 targets and routes, switch your account to use the v3 API version. This action is irreversible and will migrate your account from v1 to v3.

```sh
curl -X PATCH \
  "https://api.<region>.logs-router.cloud.ibm.com/v3/settings" \
  -H "Authorization: Bearer <IAM_TOKEN>" \
  -H 'content-type: application/json' \
  -d '{
    "api_version": 3
  }'
```
{: codeblock}

After executing this command:

1. The v1 service will shut down and stop processing logs.
2. The v3 service will become active and begin routing logs according to your new configuration.

During the migration process, there will be a brief interruption period (typically a few minutes) during which platform logs will not be received. Plan your migration accordingly to minimize the impact on your operations.
{: important}

**WARNING**: This migration is irreversible. Once you have migrated to v3, you cannot move back to v1.
{: important}

For additional information about the settings API, see [Modify settings](/apidocs/logs-router-service-api/logs-router-v3#update-settings){: external}

2. The v3 service will become active and begin routing logs according to your new configuration.

During the migration process, there will be a brief interruption period (typically a few minutes) during which platform logs will not be received. Plan your migration accordingly to minimize the impact on your operations.
{: important}

**WARNING**: This migration is irreversible. Once you have migrated to v3, you cannot move back to v1.
{: important}

For additional information about the migration API and available actions, see [Migrate from old API version to version 3](/apidocs/logs-router-service-api/logs-router-v3#migrate-actions){: external}.


## IAM Permissions for Migration
{: #v3-migration-iam}

The migration process requires specific IAM permissions. The following global actions are available for managing the migration:

- **logs-router.migration.post**: Configures and starts the migration. This is a two-step process to set up account metadata and then start the actual update of all regions. Requires Administrator role.
- **logs-router.migration.get**: Retrieves the status of the migration. Available to Administrator, Editor, Operator, and Viewer roles.
- **logs-router.migration.delete**: Deletes the generated migration plan, including all automatically created v3 targets and routes. Requires Administrator role.

For more information about IAM roles and permissions, see [IAM roles](/docs/logs-router?topic=logs-router-iam#iam-bytask).

## Conclusion
{: #v3-migration-conclusion}

You have successfully migrated from {{site.data.keyword.logs_routing_full_notm}} v1 to v3. Your platform logs are now being routed according to your v3 configuration. If you encounter any issues or need to make changes to your routing configuration, refer to the [{{site.data.keyword.logs_routing_full_notm}} v3 API documentation](/apidocs/logs-router-service-api/logs-router-v3){: external} for guidance on updating settings, targets, and routes.
