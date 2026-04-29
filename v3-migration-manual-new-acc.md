---

copyright:
  years: "2026"
lastupdated: "2026-04-29"

keywords:

subcollection: logs-router

---

{{site.data.keyword.attribute-definition-list}}

# Manual migration from V1 to V3 for new accounts (No V1 Setup)
{: #v3-migration-manual-new-acc}

If you prefer to manually configure your v3 environment, the process differs depending on whether you have an existing v1 setup or are starting fresh with a new account.

If you are setting up {{site.data.keyword.logs_routing_full_notm}} for the first time without any existing v1 configuration, follow these steps:



## Before you begin
{: #v3-migration-manual-prereqs}

1. You must have the **administrator** platform role to migrate from V1 to V3. For more information about IAM roles and permissions, see [IAM roles](/docs/logs-router?topic=logs-router-iam#iam-bytask).

2. Read about migration scenarios. For more information, see [Common Migration Scenarios](/docs/logs-router?topic=logs-router-v3-migration&interface=cli#v3-migration-scenarios).

3. Choose a migration approach. For more information, see [Migration Approaches](/docs/logs-router?topic=logs-router-v3-migration&interface=cli#v3-migration-approaches).

4. Learn about the migration states. For more information, see [Migration states](/docs/logs-router?topic=logs-router-v3-migration&interface=cli#v3-migration-states).


## Step 1: Set Primary Region and Version
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


## Step 2: Create Targets
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

For more information, see [Create target](/apidocs/logs-router-service-api/logs-router-v3#create-target){: external} and [Creating a target](/docs/logs-router?topic=logs-router-target_icl&interface=api#target_icl_api_create).


## Step 3: Create Routes
{: #v3-migration-manual-new-step-3}

Create routes to define how logs are directed to your targets.

For detailed information about route configuration options, including filtering capabilities, see [Create route](/apidocs/logs-router-service-api/logs-router-v3#create-route){: external} and [Creating a route](/docs/logs-router?topic=logs-router-route-manage&interface=api#route-manage-api-create).

Logs will start flowing once the route is defined:

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
