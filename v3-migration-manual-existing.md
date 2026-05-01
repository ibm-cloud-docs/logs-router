---

copyright:
  years: "2026"
lastupdated: "2026-04-29"

keywords:

subcollection: logs-router

---

{{site.data.keyword.attribute-definition-list}}

# Manual migration from v1 to v3 for existing accounts (With V1 Setup)
{: #v3-migration-manual-existing}

If you prefer to manually configure your v3 environment, the process differs depending on whether you have an existing v1 setup or are starting fresh with a new account.

If you have an existing v1 configuration and want to manually migrate to v3, follow these steps:

## Before you begin
{: #v3-migration-manual-existing-prereqs}

1. You must have the **administrator** platform role to migrate from V1 to V3. For more information about IAM roles and permissions, see [IAM roles](/docs/logs-router?topic=logs-router-iam#iam-bytask).

2. Read about migration scenarios. For more information, see [Common Migration Scenarios](/docs/logs-router?topic=logs-router-v3-migration&interface=cli#v3-migration-scenarios).

3. Choose a migration approach. For more information, see [Migration Approaches](/docs/logs-router?topic=logs-router-v3-migration&interface=cli#v3-migration-approaches).

4. Learn about the migration states. For more information, see [Migration states](/docs/logs-router?topic=logs-router-v3-migration&interface=cli#v3-migration-states).

During the migration process, there will be a brief interruption period (typically a few minutes) during which platform logs will not be received. Plan your migration accordingly to minimize the impact on your operations.
{: important}

**WARNING**: This migration is irreversible. Once you have migrated to v3, you cannot move back to v1.
{: important}


## Step 1: Set Primary Metadata Region
{: #v3-migration-manual-existing-metadata}

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

## Step 2: Generate a V3 configuration from your existing V1 configuration
{: #v3-migration-manual-existing-generate}

The current migration process requires that you begin the migration by generating a configuration from the existing V1 setup.  This step will create routes and targets based on the V1 targets.

```sh
curl -X POST \
  https://api.<region>.logs-router.cloud.ibm.com/v3/migrate?action=generate \
  -H "Authorization: Bearer <IAM_TOKEN>" \
  -H 'content-type: application/json'
```

 Once the routes and targets are generated, review the generated configurations.

 ## Step 3: Check Migration Status
{: #v3-migration-manual-existing-status}

After initiating the migration plan generation, you need to monitor its progress. Continue checking the status until the state changes to `PENDING_COMPLETION`.

Use the following API call to check the status:

```sh
curl -X GET \
  "https://api.<region>.logs-router.cloud.ibm.com/v3/migrate" \
  -H "Authorization: Bearer <IAM_TOKEN>"
```
{: codeblock}

The authorization bearer token must have the `logs-router.migration.get` permission. The following roles have this permission: Administrator, Editor, Operator, or Viewer role.

The API will return one of the following responses based on the current state:

- **COMPLETE state** (HTTP 200 OK):

    ```json
    {
      "version": "3",
      "state": "COMPLETE",
      "message": "Migration complete"
    }
    ```
    {: codeblock}

- **BEFORE state** (HTTP 200 OK):

    ```json
    {
      "version": "1",
      "state": "BEFORE",
      "message": "Error message with reason for previous failure"
    }
    ```
    {: codeblock}

    If the message is empty, no previous migration attempt has been made or failed.{: note}

- **IN_PROGRESS state** (HTTP 200 OK):

    ```json
    {
      "version": "1",
      "state": "IN_PROGRESS",
      "message": "The migration process is still running - check back in a few minutes"
    }
    ```
    {: codeblock}

- **PENDING_COMPLETION state** (HTTP 200 OK):

    ```json
    {
      "version": "1",
      "state": "PENDING_COMPLETION",
      "message": "Your v3 routes and targets have been created - verify the content and then use action=complete to switch your account permanently to the v3 configuration."
    }
    ```
    {: codeblock}



## Step 4: Create Targets
{: #v3-migration-manual-existing-targets}

Create target destinations for your v3 configuration:

For more information, see [Create target](/apidocs/logs-router-service-api/logs-router-v3#create-target){: external} and [Creating a target](/docs/logs-router?topic=logs-router-target_icl&interface=api#target_icl_api_create).


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


## Step 5: Create Routes
{: #v3-migration-manual-existing-routes}

Create routes to define how logs should be directed to your targets:

For detailed information about route configuration options, including filtering capabilities, see [Create route](/apidocs/logs-router-service-api/logs-router-v3#create-route){: external} and [Creating a route](/docs/logs-router?topic=logs-router-route-manage&interface=api#route-manage-api-create).

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

## Step 6: Review Generated Targets and Routes
{: #v3-migration-manual-existing-review}

Once the migration reaches the `PENDING_COMPLETION` state, you should review the automatically generated v3 targets and routes to ensure they match your requirements.

### Review targets
{: #v3-migration-manual-existing-review-targets}

To list all targets, use the following API call:

```sh
curl -X GET \
  "https://api.<region>.logs-router.cloud.ibm.com/v3/targets" \
  -H "Authorization: Bearer <IAM_TOKEN>"
```
{: codeblock}

For more information, see [List targets](/apidocs/logs-router-service-api/logs-router-v3#list-targets){: external}.

### Review routes
{: #v3-migration-manual-existing-review-routes}

To list all routes, use the following API call:

```sh
curl -X GET \
  "https://api.<region>.logs-router.cloud.ibm.com/v3/routes" \
  -H "Authorization: Bearer <IAM_TOKEN>"
```
{: codeblock}

For more information, see [List routes](/apidocs/logs-router-service-api/logs-router-v3#list-routes){: external}.


## Step 7: Complete the migration by switching to V3
{: #v3-migration-manual-existing-switch}

Once you have configured all your v3 targets and routes, switch your account to use the v3 API version.

This action is irreversible and will migrate your account from v1 to v3.
{: attention}

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

After running this command:

1. The v1 service will shut down and stop processing logs.
2. The v3 service will become active and begin routing logs according to your new configuration.



## Next
{: #v3-migration-manual-existing-next}

You have successfully migrated from {{site.data.keyword.logs_routing_full_notm}} v1 to v3. Your platform logs are now being routed according to your v3 configuration.

If you encounter any issues or need to make changes to your routing configuration, refer to the [{{site.data.keyword.logs_routing_full_notm}} v3 API documentation](/apidocs/logs-router-service-api/logs-router-v3){: external} for guidance on updating settings, targets, and routes.
