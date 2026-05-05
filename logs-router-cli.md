---

copyright:
  years: 2023, 2026
lastupdated: "2026-05-05"

subcollection: logs-router

keywords:

content-type: cli-docs

---

{{site.data.keyword.attribute-definition-list}}



# {{site.data.keyword.logs_routing_full_notm}} CLI
{: #logs-router-cli}

The {{site.data.keyword.cloud}} command-line interface (CLI) provides extra capabilities for service offerings. This information describes how you can use the CLI to define and manage the {{site.data.keyword.logs_routing_full_notm}} service by using the CLI.
{: shortdesc}



## Before you begin
{: #mr-prereq}

Complete the following steps:

1. Install the [{{site.data.keyword.cloud_notm}} CLI](/docs/cli?topic=cli-getting-started).

2. Install and configure the [{{site.data.keyword.logs_routing_full_notm}} CLI](/docs/logs-router?topic=logs-router-logs-router-cli-config).

You're notified on the command line when updates to the {{site.data.keyword.cloud_notm}} CLI and plug-ins are available. Be sure to keep your CLI up to date so that you can use the most current commands. You can view the current version of all installed plug-ins by running `ibmcloud plugin list`.
{: note}


## ibmcloud logs-router route create
{: #route-create-cli}

Use this command to create a route that defines how data is routed to {{site.data.keyword.logs_routing_full_notm}} targets in an account from one or more regions. A route can be managed in the account where it is created or can be managed by the enterprise parent account.

Targets must be {{site.data.keyword.logs_full_notm}} instances.
{: restriction}

```sh
ibmcloud logs-router route create --name ROUTE_NAME ( --rules RULES | @RULES-FILE) [--managed-by MANAGED-BY] [--output FORMAT]
```
{: pre}

### Command options
{: #route-create-options}

`--name ROUTE_NAME`
:   The name to be given to the route.

    The maximum length is 1000 characters. The minimum length is 1 character.

    The name cannot include any special characters other than `(space) - . _ :`.

    Do not include any personal identifying information (PII) in any resource names.
    {: important}

`--rules RULES | @RULES-FILE`
:   Routing rules that will be evaluated in the order in which they are configured.

    Must be set to a JSON string enclosed in single quotation marks or a path to a JSON file.

    The maximum length is 10 rules. The minimum length is 1 rule.

    For example, you can set a JSON string as follows or use @filepath to pass the json:

    ```json
    --rules '[{"action": "send", "targets":[{"id": "11111111-1111-1111-1111-111111111111"}], "inclusion_filters":[{"operand": "location","operator": "is","values": ["us-south","eu-de"]}]}]'
    ```
    {: codeblock}

    You can format the JSON file as follows to route all data to the targets that are configured in the rule:

    ```json
    [
      {
        "targets": [{"id":"ID1"},{"id":"ID2"}]
      }
    ]
    ```
    {: codeblock}

    The rule definition can optionally include inclusion filters. You can format the JSON file as follows:

    ```json
    [{
        "action": "send",
        "targets": [{
          "id":"11111111-1111-1111-1111-111111111111"
        }],
        "inclusion_filters": [
            {
              "operand": "location",
              "operator": "in",
              "values": [
                "us-south",
                "eu-de"
              ]
            }
        ]
     }]
    ```
    {: codeblock}

    Where:

    `action`
    :   Action defines whether {{site.data.keyword.logs_routing_full_notm}} includes or excludes logs on the route. Two actions are supported: `send` and `drop`. If not specified, the default action is to send the logs.

        `send`
        :   Logs are sent, based on the routing rule, on the defined route.

        `drop`
        :   Logs are excluded, based on the routing rule, when logs are sent on the defined route.

    `targets`
    :   Targets define the {{site.data.keyword.logs_full_notm}} destinations where logs are routed. It is a comma-separated list of target IDs.

    `inclusion_filters`
    :   Inclusion filters define what data is routed.

        You can configure a comma-separated list of filters.

        A filter is defined by an operand, and operator, and values.

        You can define a maximum of 7 filters per rule.

    `operand`
    :   Operand is the name of the property on which the `operator` is run.

        Valid operands are: `location`.

    `operator`
    :   Two operators are supported: `in` and `is`.

        `in`
        :   The value of the operand property is compared to a list of values.

             You can define up to 20 values.

        `is`
        :   The value of the operand property is compared to a single value.

            When using `is`, only 1 value can be specified.

    `values`
    :   A string, or an array of strings, to be compared with the `operand` property to determine whether the log is routed or not.

        When the `is` `operator` is used, `values` must include a single string.

        When the `in` `operator` is used, `values` can include multiple strings in an array.

`--managed-by`
:   Identifies how a route is managed.

    A route can be managed locally in the account where is created by setting the default value `account` or can be managed by the enterprise account by setting the value `enterprise`.

    Valid values are: `enterprise`, or `account`.

    The default value is 'account'.

    You cannot change this value after the route is created.
    {: attention}

`--output FORMAT`
:   If `JSON` is specified, output is returned in JSON format. If `JSON` is not specified, output is returned in a tabular format.

`help` | `--help` | `-h`
:   List options available for the command.


## ibmcloud logs-router route update
{: #route-update-cli}

Use this command to update a route that defines how data is routed to {{site.data.keyword.logs_routing_full_notm}} targets in an account from one or more regions.

Any specified value that is different from when the route was originally created is updated to the value specified in the command. Any configuration previously deployed and not included in the update request is removed.
{: attention}

```sh
ibmcloud logs-router route update --id ID [--name ROUTE_NAME] [--rules RULES | @RULES-FILE ] [--output FORMAT]
```
{: pre}

### Command options
{: #route-update-options}

`--id ID`
:   The ID of the route.

    The ID is a v4 UUID that uniquely identifies the route.

    The maximum length is 1028 characters. The minimum length is 24 characters.

`--name ROUTE_NAME`
:   The new name to be given to the route (optional).

    Do not include any personal identifying information (PII) in any resource names.
    {: important}

`--rules RULES | @RULES-FILE`
:  Routing rules that will be evaluated in the order in which they are configured.

    Must be set to a JSON string enclosed in single quotation marks or a path to a JSON file.

    The maximum length is 10 rules. The minimum length is 1 rule.

    Keep your current configuration in addition to modifications that you must make. Any existing rule configuration that is not included in the update is deleted.
    {: attention}

    For example, see the sample of a JSON formatted rule definition or use @filepath to pass the json:

    ```json
    --rules '[{"action": "send", "targets":[{"id": "11111111-1111-1111-1111-111111111111"}], "inclusion_filters":[{"operand": "location","operator": "is","values": ["us-south","eu-de","eu-gb"]}]}]'
    ```
    {: codeblock}

    You can format the JSON file as follows to route all data to the targets that are configured in the rule:

    ```json
    [
      {
        "targets": [{"id":"ID1"},{"id":"ID2"},{"id":"ID3"}]
      }
    ]
    ```
    {: codeblock}

    The rule definition can optionally include inclusion filters. You can format the JSON file as follows:

    ```json
    [{
        "action": "send",
        "targets": [{
          "id":"11111111-1111-1111-1111-111111111111"
        }],
        "inclusion_filters": [
            {
              "operand": "location",
              "operator": "in",
              "values": [
                "us-south",
                "eu-de",
                "eu-gb"
              ]
            }
        ]
     }]
    ```
    {: codeblock}

    `action`
    :   Action defines whether {{site.data.keyword.logs_routing_full_notm}} includes or excludes logs on the route. Two actions are supported: `send` and `drop`. If not specified, the default action is to send the logs.

        `send`
        :   Logs are sent, based on the routing rule, on the defined route.

        `drop`
        :   Logs are excluded, based on the routing rule, when logs are sent on the defined route.

    `targets`
    :   Targets define the {{site.data.keyword.logs_full_notm}} destinations where logs are routed. It is a comma-separated list of target IDs.

    `inclusion_filters`
    :   Inclusion filters define what data is routed.

        You can configure a comma-separated list of filters.

        A filter is defined by an operand, and operator, and values.

        You can define a maximum of 7 filters per rule.

    `operand`
    :   Operand is the name of the property on which the `operator` is run.

        Valid operands are: `location`.

    `operator`
    :   Two operators are supported: `in` and `is`.

        `in`
        :   The value of the operand property is compared to a list of values.

             You can define up to 20 values.

        `is`
        :   The value of the operand property is compared to a single value.

            When using `is`, only 1 value can be specified.

    `values`
    :   A string, or an array of strings, to be compared with the `operand` property to determine whether the log is routed or not.

        When the `is` `operator` is used, `values` must include a single string.

        When the `in` `operator` is used, `values` can include multiple strings in an array.

`--output FORMAT`
:   If `JSON` is specified, output is returned in JSON format. If `JSON` is not specified, output is returned in a tabular format.

`help` | `--help` | `-h`
:   List options available for the command.


## ibmcloud logs-router route delete
{: #route-rm-cli}

Use this command to delete an {{site.data.keyword.logs_routing_full_notm}} route.

```sh
ibmcloud logs-router route delete --id ID [--force]
```
{: pre}

### Command options
{: #route-rm-options}

`--id ID`
:   The ID of the route to be deleted.


`--force` or `--f`
:   Deletes the route without providing the user with any additional prompt.


`help` | `--help` | `-h`
:   List options available for the command.


## ibmcloud logs-router route get
{: #route-get-cli}

Use this command to get configuration information about an {{site.data.keyword.logs_routing_full_notm}} route.

```sh
ibmcloud logs-router route get --id ID
```
{: pre}

### Command options
{: #route-get-options}

`--id ID`
:   The ID of the route.



`help` | `--help` | `-h`
:   List options available for the command.

## ibmcloud logs-router route list
{: #route-ls-cli}

Use this command to list all the configured routes for {{site.data.keyword.logs_routing_full_notm}} in the account.

```sh
ibmcloud logs-router route list
```
{: pre}

### Command options
{: #route-ls-options}



`help` | `--help` | `-h`
:   List options available for the command.

## ibmcloud logs-router target create
{: #target-create-cli}

Use this command to create a target to be used to configure a destination for platform metrics. You can send your platform logs from all regions to a single target, different targets or multiple targets. One target per region is not required. You can define up to 16 targets per account.

Targets must be {{site.data.keyword.logs_full_notm}} instances.
{: restriction}

```sh
ibmcloud logs-router target create --name NAME --destination-crn DESTINATION-CRN [--region REGION] [--managed-by MANAGED-BY]
```
{: pre}

### Command options
{: #target-create-options-at}

`--region REGION`
:   Name of the region, for example, `us-south` or `eu-gb`. If not specified, the region that is logged into, or targeted, is used. This is optional.

`--name NAME`
:   The name to be given to the target.

    Do not include any personal identifying information (PII) in any resource names.
    {: important}

`--destination-crn DESTINATION-CRN`
:   The CRN of the service instance or resource to receive the logs sent by {{site.data.keyword.logs_routing_full_notm}}. Ensure you have a service authorization between {{site.data.keyword.logs_routing_full_notm}} and your {{site.data.keyword.cloud_notm}} resource. For more information, see [Managing authorizations to grant access between services.](/docs/metrics-router?topic=metrics-router-iam-service-auth).

`[--managed-by MANAGED-BY]`
This is optional. Identifies who manages this target. Optional at create time. If set to 'enterprise', the target becomes enterprise-managed and can only be modified by identities authorized for the enterprise actions. If omitted or set to 'account', the target is managed by the child account. This value is immutable after creation. Allowable values are:enterprise, account.

`help` | `--help` | `-h`
:   List options available for the command.

## ibmcloud logs-router target update
{: #target-update-cli}

Use this command to update a target to be used to configure a destination for platform metrics.

Targets must be {{site.data.keyword.logs_full_notm}} instances.
{: restriction}

```sh
ibmcloud logs-router target update --id ID [--name NAME] [--destination-crn DESTINATION-CRN]
```
{: pre}

### Command options
{: #target-update-options}

`--id ID`
The id of the target to be created.

`--name NAME`
:   The new name to be given to the target.

    Do not include any personal identifying information (PII) in any resource names.
    {: important}

`--destination-crn DESTINATION_CRN`
:   The CRN of the service instance or resource to receive the logs sent by {{site.data.keyword.logs_routing_full_notm}}. Ensure you have a service authorization between {{site.data.keyword.logs_routing_full_notm}} and your {{site.data.keyword.cloud_notm}} resource. For more information, see [Managing authorizations to grant access between services.](/docs/logs-router?topic=logs-router-iam-service-auth).



`help` | `--help` | `-h`
:   List options available for the command.

## ibmcloud logs-router target delete
{: #target-delete-cli}

Use this command to delete a target.

```sh
ibmcloud logs-router target delete --id ID
```
{: pre}

### Command options
{: #target-rm-options-cos}

`--id ID`
:   The ID of the target.


`--force` | `-f`
:   Deletes the target without providing the user with any additional prompt.


`help` | `--help` | `-h`
:   List options available for the command.

## ibmcloud logs-router target get
{: #target-get-cli}

Use this command to get information about an {{site.data.keyword.logs_routing_full_notm}} target.

```sh
ibmcloud logs-router target get --id ID
```
{: pre}

### Command options
{: #target-get-options}

`--id ID`
:   The ID of the target.



`help` | `--help` | `-h`
:   List options available for the command.

## ibmcloud logs-router target list
{: #target-list-cli}

Use this command to list all the configured targets for {{site.data.keyword.logs_routing_full_notm}}.

```sh
ibmcloud logs-router target list
```
{: pre}

### Command options
{: #target-list-opts}



`help` | `--help` | `-h`
:   List options available for the command.

## ibmcloud logs-router setting get
{: #settings-get-cli}

Use this command to get the settings for the {{site.data.keyword.logs_routing_full_notm}} account level configurations.

```sh
ibmcloud logs-router setting get
```
{: pre}

### Command options
{: #settings-get-options}



`help` | `--help` | `-h`
:   List options available for the command.

## ibmcloud logs-router setting update
{: #settings-update-cli}

Use this command to modify current settings such as the default targets, permitted target regions, primary and secondary metadata regions in {{site.data.keyword.logs_routing_full_notm}}. Any value that is different from when the target was originally created is updated to the value specified in the command. If no options are specified, the current settings are returned.

Before you disable public endpoints by setting `--private-api-endpoint-only TRUE`, make sure your account has access to the private endpoint.  You can do this by running the command `ibmcloud account show`.  If `VRF Enabled` is `true` and `Service Endpoint Enabled` is `true` then you have access to the private endpoint. If you do not have access to the private endpoint, you cannot enable the public endpoint. Private endpoint access is required to enable the public endpoint.
{: important}

```sh
ibmcloud logs-router setting update [--default-targets DEFAULT-TARGETS] [--permitted-target-regions PERMITTED-TARGET-REGIONS] [--primary-metadata-region PRIMARY-METADATA-REGION] [--backup-metadata-region BACKUP-METADATA-REGION] [--private-api-endpoint-only=PRIVATE-API-ENDPOINT-ONLY]
```
{: pre}

### Command options
{: #settings-update-options}



`--api-version`

:   API version for IBM Cloud Logs Routing service in the IBM Cloud account. Valid values are 1 and 3. The default value is 1. You can update from 1 to 3 to migrate your service configuration only if you created valid targets and tenants in V1. Notice that once you make this update, there is no option to got back to version 1. For more information, see https://ibm.biz/lr-v3migration.




`--backup-metadata-region`

:   Backup region that is used to backup the IBM Cloud Logs Routing metadata. The maximum length is 256 characters. The minimum length is 3 characters.




`--default-targets`

:   Targets that receive logs that are not explicitly managed in the IBM Cloud Logs Routing account's routing rules. You can define up to 2 targets. The minimum number of targets is 0.




`--permitted-target-regions` 

:  Comma separated list of regions that are supported by {{site.data.keyword.logs_routing_full_notm}} where users can create targets in the account. For more information, see https://cloud.ibm.com/docs/logs-router?topic=logs-router-locations. The maximum length is 16 regions. The minimum length is 0 region. 




`--primary-metadata-region` 

:  Primary region that is used to backup the IBM Cloud Logs Routing metadata. The maximum length is 256 characters. The minimum length is 3 characters.




`--private-api-endpoint-only` 

:  Endpoint connectivity supported. Set this value to "false" to access the API through the public network. Set this value to "true" to access the API through the private network only.




 `help` | `--help` | `-h`
:   List options available for the command.

## ibmcloud logs-router migrate complete
{: #migrate-update-cli}

Use this command to complete the migration logs routing versio 1 to version 3. The migration status must be PENDING_COMPLETION to use this command.

```sh
ibmcloud logs-router migrate complete
```
{: pre}

### Command options
{: #migrate-complete-options}

`help` | `--help` | `-h`
:   List options available for the command.


## ibmcloud logs-router migrate generate
{: #migrate-generate-cli}

Use this command to start the async migration of logs routing from version 1 to version 3.
The migration status must be BEFORE to use this command.

```sh
ibmcloud logs-router migrate generate
```
{: pre}

### Command options
{: #migrate-generate-options}

`help` | `--help` | `-h`
:   List options available for the command.


## ibmcloud logs-router migrate delete
{: #migrate-delete-cli}

Use this command to delete all logs routing version 3 routes and targets and reset the migration status to BEFORE.
Only allowed when state is PENDING_COMPLETION or BEFORE.

```sh
ibmcloud logs-router migrate delete
```
{: pre}

### Command options
{: #migrate-delete-options}

--force` | `-f`
:   Deletes the operation without confirmation.

`help` | `--help` | `-h`
:   List options available for the command.


## ibmcloud logs-router migrate status
{: #migrate-status-cli}

Use this command to retrieve the status logs routing migration state and version information.
The possible states are: COMPLETE, BEFORE, IN_PROGRESS, and PENDING_COMPLETION.

```sh
ibmcloud logs-router migrate status
```
{: pre}

### Command options
{: #migrate-status-options}

`help` | `--help` | `-h`
:   List options available for the command.
