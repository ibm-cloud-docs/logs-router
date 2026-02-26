---

copyright:
  years: "2026"
lastupdated: "2026-02-26"

keywords:

subcollection: logs-router

---

{{site.data.keyword.attribute-definition-list}}

# Migration Guide: Transitioning from v1 to v3
{: #v3-migration}

This feature is available to select customers.
{: preview}

This guide provides step-by-step instructions for migrating your {{site.data.keyword.logs_routing_full_notm}} service from version 1 (v1) to version 3 (v3). While v1 provides a regional concept, v3 offers a global approach with routes and filters to adjust routing of platform logs to your needs. The migration process involves configuring your new v3 environment and then switching from v1 to v3.
{: shortdesc}

Note that there will be a brief service interruption (approximately a few minutes) during the final migration step where platform logs are not received.
{: important}

**WARNING**: This migration is irreversible. Once you have migrated to v3, you cannot move back to v1.
{: important}

## Common Migration Scenarios
{: #v3-migration-scenarios}

Before beginning your migration, it is important to understand which scenario best matches your current logging architecture. The following three scenarios represent the most common logging configurations and will help you determine the appropriate setup for your v3 environment.

### Scenario 1: Centralized Logging
{: #v3-migration-scenarios-1}

In a centralized logging configuration, all platform logs for your entire account are consolidated into a single {{site.data.keyword.logs_full_notm}} instance.

This approach simplifies log management by providing a unified view of all platform activities across your account.

To implement this scenario in v3, you will need to create one target pointing to your centralized {{site.data.keyword.logs_full_notm}} instance and configure one route with a wildcard rule to capture all platform logs regardless of their source region.

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

## Section 1: Configure Your New v3 Setup
{: #v3-migration-s1-configure-v3}

Before migrating from v1 to v3, you must configure your new v3 environment. This section guides you through the required manual setup steps.

### Step 1.1: Set the Primary Metadata Region
{: #v3-migration-s1-configure-v3-step-1}

The first step is to configure the primary metadata region for your v3 setup. This setting determines in which region your routing metadata will be stored. Select the region that best meets your data residency or compliance requirements.

To set the primary metadata region, run the following API call:

```sh
curl -X PATCH \
  https://api.<region>.logs-router.cloud.ibm.com/v3/settings \
  -H "Authorization: Bearer <IAM_TOKEN>" \
  -H 'accept: application/json' \
  -d '{
    "primary_metadata_region": "<region>"
  }'
```
{: codeblock}

Set `<region>` to your desired region such as `us-south`, or `eu-de`.

For detailed information about this API endpoint and available parameters, see [Modify settings](/apidocs/logs-router-service-api/logs-router-v3#update-settings){: external}


### Step 1.2: Create a Target for Platform Logs
{: #v3-migration-s1-configure-v3-step-2}

Next, you need to create a target destination where your platform logs will be delivered. A target represents the endpoint that will receive your log data.

Run the following API call to create a target:

```sh
curl -X POST \
  https://api.<region>.logs-router.cloud.ibm.com/v3/targets \
  -H "Authorization: Bearer <IAM_TOKEN>" \
  -H 'content-type: application/json' \
  -d '{
    "name": "<NAME>",
    "destination_crn": "<CRN>"
  }'
```
{: codeblock}

Where
- `<NAME>` is a descriptive name for your target
- `<CRN>` is the Cloud Resource Name of your destination {{site.data.keyword.logs_full_notm}} instance

The API response will include a target ID. Make sure to save this ID, as you will need it in the next step.

For more information about creating targets and available configuration options, see [Create target](/apidocs/logs-router-service-api/logs-router-v3#create-target){: external}


### Step 1.3: Create a Route to the Target
{: #v3-migration-s1-configure-v3-step-3}

After creating your target, you must create a route that defines how logs should be directed to that target. Routes contain rules that determine which logs are sent to which targets.

Run the following API call to create a route that delivers platform logs from all regions to a central target:

```sh
curl -X POST \
  https://api.<region>.logs-router.cloud.ibm.com/v3/routes \
  -H "Authorization: Bearer <IAM_TOKEN>" \
  -H 'content-type: application/json' \
  -d '{
    "name": "<NAME>",
    "rules": [
      {
        "action": "send",
        "targets": [
          {
            "id": "<ID FROM PREV STEP>"
          }
        ]
      }
    ]
  }'
```
{: codeblock}

Where
- `<NAME>` is a descriptive name for your route
- `<ID FROM PREV STEP>` is the target ID that defines the {{site.data.keyword.logs_full_notm}} instance where platform logs are routed.

You can add additional filters and routing rules to customize log delivery behavior. For detailed information about route configuration options, including filtering capabilities, see [Create route](/apidocs/logs-router-service-api/logs-router-v3#create-route){: external}


## Section 2: Migrate from v1 to v3
{: #v3-migration-s2-migrate}

Once you have completed the v3 setup in Section 1, you are ready to perform the actual migration from v1 to v3.


### Step 2.1: Initiate the Migration
{: #v3-migration-s2-migrate-s1}

To migrate from v1 to v3, you need to instruct the v1 service to switch over to the v3 configuration. This is accomplished by calling the migration API endpoint.

Run the following API call to complete the migration:

```sh
curl -X POST \
  https://api.<region>.logs-router.cloud.ibm.com/v3/migrate?action=complete \
  -H "Authorization: Bearer <IAM_TOKEN>" \
  -H 'content-type: application/json'
```
{: codeblock}

The authorization bearer token must have the IAM roles `Manager` and `Administrator` for the {{site.data.keyword.logs_routing_full_notm}} service in order to complete the call successfully. For more information, see [IAM roles](/docs/logs-router?topic=logs-router-iam#iam-bytask).

For additional information about the migration API and available actions, see [Migrate from old API version to version 3](/apidocs/logs-router-service-api/logs-router-v3#migrate-actions){: external}.


### Step 2.2: Post-Migration Behavior
{: #v3-migration-s2-migrate-s2}

After executing the migration API call, the following changes will occur:

1. The v1 service will shut down and stop processing logs.
2. The v3 service will become active and begin routing logs according to your new configuration.

During the migration process, there will be a brief interruption period (typically a few minutes) during which platform logs will not be received. Plan your migration accordingly to minimize the impact on your operations.
{: important}


## Conclusion
{: #v3-migration-conclusion}

You have successfully migrated from {{site.data.keyword.logs_routing_full_notm}} v1 to v3. Your platform logs are now being routed according to your v3 configuration. If you encounter any issues or need to make changes to your routing configuration, refer to the [{{site.data.keyword.logs_routing_full_notm}} v3 API documentation](/apidocs/logs-router-service-api/logs-router-v3){: external} for guidance on updating settings, targets, and routes.
