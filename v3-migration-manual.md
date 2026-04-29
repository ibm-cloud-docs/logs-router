---

copyright:
  years: "2026"
lastupdated: "2026-04-29"

keywords:

subcollection: logs-router

---

{{site.data.keyword.attribute-definition-list}}

# Migration Guide: Manual Migration from v1 to v3
{: #v3-migration-manual}

If you prefer to manually configure your v3 environment, the process differs depending on whether you have an existing v1 setup or are starting fresh with a new account.


## Before you begin
{: #v3-migration-manual-prereqs}

1. You must have the **administrator** platform role to migrate from V1 to V3. For more information about IAM roles and permissions, see [IAM roles](/docs/logs-router?topic=logs-router-iam#iam-bytask).

2. Read about migration scenarios. For more information, see [Common Migration Scenarios](/docs/logs-router?topic=logs-router-v3-migration&interface=cli#v3-migration-scenarios).

3. Choose a migration approach. For more information, see [Migration Approaches](/docs/logs-router?topic=logs-router-v3-migration&interface=cli#v3-migration-approaches).

4. Learn about the migration states. For more information, see [Migration states](/docs/logs-router?topic=logs-router-v3-migration&interface=cli#v3-migration-states).

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



## Conclusion
{: #v3-migration-conclusion}

You have successfully migrated from {{site.data.keyword.logs_routing_full_notm}} v1 to v3. Your platform logs are now being routed according to your v3 configuration. If you encounter any issues or need to make changes to your routing configuration, refer to the [{{site.data.keyword.logs_routing_full_notm}} v3 API documentation](/apidocs/logs-router-service-api/logs-router-v3){: external} for guidance on updating settings, targets, and routes.
