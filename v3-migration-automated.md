---

copyright:
  years: "2026"
lastupdated: "2026-04-29"

keywords:

subcollection: logs-router

---

{{site.data.keyword.attribute-definition-list}}

# Migration Guide: Automated Migration from v1 to v3
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



## Conclusion
{: #v3-migration-conclusion}

You have successfully migrated from {{site.data.keyword.logs_routing_full_notm}} v1 to v3. Your platform logs are now being routed according to your v3 configuration. If you encounter any issues or need to make changes to your routing configuration, refer to the [{{site.data.keyword.logs_routing_full_notm}} v3 API documentation](/apidocs/logs-router-service-api/logs-router-v3){: external} for guidance on updating settings, targets, and routes.
