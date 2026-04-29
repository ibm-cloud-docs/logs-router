---

copyright:
  years:  2023, 2026
lastupdated: "2026-04-29"

keywords:

subcollection: logs-router

---

{{site.data.keyword.attribute-definition-list}}



# Managing routes
{: #route-manage}

You can manage routes in your account by using the {{site.data.keyword.logs_routing_full_notm}} UI, the {{site.data.keyword.logs_routing_full_notm}} CLI, the {{site.data.keyword.logs_routing_full_notm}} REST API V3, and the {{site.data.keyword.logs_routing_full_notm}} Terraform provider. A route defines the rules that indicate what platform logs are routed in a region and where to route them.
{: shortdesc}

For more information on {{site.data.keyword.logs_routing_full_notm}} routes, see [routes](/docs/logs-router?topic=logs-router-routes&interface=cli).


## IAM Access
{: #route-manage-iam}

You must have the correct IAM permissions to manage routes. For information, see [Managing IAM access.](/docs/logs-router?topic=logs-router-iam)



## Creating a route using the UI
{: #route-create-ui}
{: ui}

Complete the following steps to create a route using the UI.

1. [Log in to your {{site.data.keyword.cloud_notm}} account](https://cloud.ibm.com/login){: external}.

2. Click the **Menu** icon ![Menu icon](../icons/icon_hamburger.svg) &gt; **Observability**.

3. Click **Logging > Routing**.

4. In the *Routes* tab, click **Create**.

5. In *Routing rules* section, modify the *Action* for *Rule 1*:

    Select an *Action* for *Rule 1*:

    - Select **Send logs from** for the rule to route platform logs to the associated targets.

    - Select **Drop logs from** for the rule to drop platform logs matching this rule.

    Add the [inclusion filters](/docs/logs-router?topic=logs-router-route_rules_definitions&interface=ui#route_rules_definitions_filters) to determine the platform logs routed to the targets specified in the rule.

    - Select a condition, and specify the location values to be matched by the inclusion filter.

    - To add multiple inclusion filters, click **Add filter** to add additional filters.

    Add the [targets](/docs/logs-router?topic=logs-router-target&interface=ui) to associate with the rule by selecting a target from the list. If you do not have a target defined, click **Add target** to create a new one.

6. Click **Add rule** to add additional rules to the route.

    The order of route rules affects the routing behavior. Rules are processed in order and once a rule is matched, the subsequent rules are not processed.

    The order of the routing rules can be changed by clicking the up and down arrows to the right of each rule definition.

    If you need to remove a rule or filter, click the **Remove** icon associated with the rule or filter.

    You can configure up to 10 rules per route.
    {: note}

7. Click **Next**.

8. Enter a name for the route.

9. Review the route definition ensuring the order of the rules is as intended.

    For more information, see [Understanding routing precedence](/docs/logs-router?topic=logs-router-routes_precedence).

10. Click **Create**.



## Updating a route using the UI
{: #route-update-ui}
{: ui}

Complete the following steps to update a route using the UI.

1. [Log in to your {{site.data.keyword.cloud_notm}} account](https://cloud.ibm.com/login){: external}.

2. Click the **Menu** icon ![Menu icon](../icons/icon_hamburger.svg) &gt; **Observability**.

3. Click **Logging > Routing**.

4. In the *Routes* tab, determine which route to update and click the ![Actions icon](../icons/action-menu-icon.svg "Actions").

5. Click **Rename** to rename the route.

6. Click **Edit** to update the route rules.

    In **Routing rules**, modify the **Action** for the rule:

    - Select `Send` for the rule to route platform logs to the associated targets.

    - Select `Drop` for the rule to drop platform logs matching this rule.

    Add or modify the [inclusion filters](/docs/logs-router?topic=logs-router-route_rules_definitions&interface=ui#route_rules_definitions_filters) to determine the platform logs routed to the targets specified in the rule.

    Select the desired filter and condition and specify the value to be matched by the inclusion filter.

    To add multiple inclusion filters, click **Add filter** to add additional filters.

7. Modify the [target](/docs/logs-router?topic=logs-router-target&interface=ui) to associate with the rule by selecting a target from the list. If you do not have a target defined, click **Add target** to create a new target.

8. Click **Add rule** to add additional rules to the route.

    The order of route rules affects the routing behavior. Rules are processed in order and once a rule is matched, the subsequent rules are not processed.

    The order of the routing rules can be changed by clicking the up and down arrows to the right of each rule definition.

    If you need to remove a rule or filter, click the **Remove** icon associated with the rule or filter.

    You can configure up to 10 rules per route.
    {: note}

9. Click **Update** to make changes to your route.


## Viewing a route using the UI
{: #route-view-ui}
{: ui}

Complete the following steps to view a route using the UI.

1. [Log in to your {{site.data.keyword.cloud_notm}} account](https://cloud.ibm.com/login){: external}.

2. Click the **Menu** icon ![Menu icon](../icons/icon_hamburger.svg) &gt; **Observability**.

3. Click **Logging > Routing**.

4. In the *Routes* tab, you can see the list of configured routes.

    The order of the routes does not affect the routing behavior since they are processed independently.

    Each route displays the route name and rules.

    The routes page also displays **Routing guidance** with additional information about configuring routing.


## Deleting a route using the UI
{: #route-delete-ui}
{: ui}

Complete the following steps to delete a route using the UI.

1. [Log in to your {{site.data.keyword.cloud_notm}} account](https://cloud.ibm.com/login){: external}.

2. Click the **Menu** icon ![Menu icon](../icons/icon_hamburger.svg) &gt; **Observability**.

3. Click **Logging > Routing**.

4. In the *Routes* tab, determine which route to delete and click the ![Actions icon](../icons/action-menu-icon.svg "Actions").

5. Click **Delete** to delete the entire route. You must enter the route name before the route is deleted.



## CLI prerequisites
{: #route-manage-cli-prereqs}
{: cli}

Before you use the CLI to manage routes, complete the following steps:

1. [Install the {{site.data.keyword.cloud_notm}} CLI](/docs/cli?topic=cli-install-ibmcloud-cli).

2. [Install the {{site.data.keyword.logs_routing_full_notm}} CLI](/docs/logs-router?topic=logs-router-logs-router-cli-config).

3. Log in to {{site.data.keyword.cloud_notm}}. Run the following command: [ibmcloud login](/docs/cli?topic=cli-ibmcloud_cli#ibmcloud_login)


## Creating a route by using the CLI
{: #route-manage-cli-create}
{: cli}

Use this command to create a route.

Route names are unique in the account.
{: note}

```sh
ibmcloud logs-router route create --name NAME --rules RULES | @RULES-FILE [--output FORMAT] [--force]
```
{: pre}

### Command options
{: #route-manage-cli-create-cmd}

`--name ROUTE_NAME`
:   The name to be given to the route.

    The maximum length is 1000 characters.

    The minimum length is 1 character.

    The name cannot include any special characters other than empty space, `-`, `.`, `_`, `:`.

    Do not include any personal identifying information (PII) in any resource names.
    {: important}

`--rules RULES`
:   JSON string or a path to a JSON file prefix with `@` that define how platform logs are routed.

    Rules are evaluated in the order in which they are configured.

    The maximum length is 10 rules.

    For more information, see [Defining routing rules](/docs/logs-router?topic=logs-router-route_rules_definitions).

`--output FORMAT`
:   Currently supported format is JSON. If specified, output will be returned in JSON format.  If `JSON` is not specified, output will be returned in a tabular format.

`help` | `--help` | `-h`
:   List options available for the command.

### Example
{: #route-manage-cli-create-sample}

The following is an example using the `ibmcloud logs-router route create` command.

```text
ibmcloud logs-router route create --name route3 --rules '[{"action": "send", "targets":[{"id":"0b4e6aa9-257c-4a3a-ae42-9a36a5b3adc7"},{"id":"6f9137b3-2bb9-4724-851b-153e23c82d80"}],"inclusion_filters": [{"operand": "location", "operator": "in", "values": ["us-south","eu-de"]}]}]'

```
{: codeblock}

The output looks as follows:

```text
Id           cfa2be32-979d-4e99-a2a2-e68e65e9b8cc
Name         route3
Rules
             action              send
             inclusion_filters
                                 operand    location
                                 operator   in
                                 values     [us-south, eu-de]

             targets
                                 id            0b4e6aa9-257c-4a3a-ae42-9a36a5b3adc7
                                 name          cl-target2
                                 target_type   cloud-logs

                                 id            6f9137b3-2bb9-4724-851b-153e23c82d80
                                 name          cl-platform-logs-sydney-113
                                 target_type   cloud-logs


Created At   2026-04-29T12:59:25.764Z
Updated At   2026-04-29T12:59:25.764Z
Managed By   account

```
{: screen}

Another example:

```text
ibmcloud logs-router route create --name route4 --rules '[{"action": "send", "targets":[{"id":"0b4e6aa9-257c-4a3a-ae42-9a36a5b3adc7"},{"id":"6f9137b3-2bb9-4724-851b-153e23c82d80"}],"inclusion_filters": [{"operand": "location", "operator": "in", "values": ["us-south","eu-de"]}]},{"action": "send", "targets":[{"id":"0b4e6aa9-257c-4a3a-ae42-9a36a5b3adc7"},{"id":"6f9137b3-2bb9-4724-851b-153e23c82d80"}],"inclusion_filters": [{"operand": "location", "operator": "is", "values": ["us-east"]}]}]'
```
{: codeblock}

The output looks as follows:

```text
Id           6bb85c44-7187-4e45-b850-c912ddd2d0bf
Name         route4
Rules
             action              send
             inclusion_filters
                                 operand    location
                                 operator   in
                                 values     [us-south, eu-de]

             targets
                                 id            0b4e6aa9-257c-4a3a-ae42-9a36a5b3adc7
                                 name          cl-target2
                                 target_type   cloud-logs

                                 id            6f9137b3-2bb9-4724-851b-153e23c82d80
                                 name          cl-platform-logs-sydney-113
                                 target_type   cloud-logs


             action              send
             inclusion_filters
                                 operand    location
                                 operator   is
                                 values     [us-east]

             targets
                                 id            0b4e6aa9-257c-4a3a-ae42-9a36a5b3adc7
                                 name          cl-target2
                                 target_type   cloud-logs

                                 id            6f9137b3-2bb9-4724-851b-153e23c82d80
                                 name          cl-platform-logs-sydney-113
                                 target_type   cloud-logs


Created At   2026-04-29T13:01:11.685Z
Updated At   2026-04-29T13:01:11.685Z
Managed By   account

```
{: screen}


## Updating a route using the CLI
{: #route-manage-cli-update}
{: cli}

Use this command to update a route. Any specified value that is different from when the route was originally created will be updated to the value specified in the command.

```sh
ibmcloud logs-router route update --id ROUTE [--name NAME] [--rules RULES | @RULES-FILE] [--output FORMAT] [--force]
```
{: pre}

### Command options
{: #route-manage-cli-update-cmd}

`--id ROUTE`
:   The ID of the route.

`--name ROUTE_NAME`
:   The name to be given to the route.

    The maximum length is 1000 characters.

    The minimum length is 1 character.

    The name cannot include any special characters other than empty space, `-`, `.`, `_`, `:`.

    Do not include any personal identifying information (PII) in any resource names.
    {: important}

`--rules RULES`
:   JSON string or a path to a JSON file prefix with `@` that define how platform logs are routed.

    Rules are evaluated in the order in which they are configured.

    The maximum length is 10 rules.

    For more information, see [Defining routing rules](/docs/logs-router?topic=logs-router-route_rules_definitions).

`--output FORMAT`
:   Currently supported format is JSON. If specified, output will be returned in JSON format.  If `JSON` is not specified, output will be returned in a tabular format.

`help` | `--help` | `-h`
:   List options available for the command.

### Example
{: #route-manage-cli-update-sample}

The following is an example using the `ibmcloud logs-router route update --id 6bb85c44-7187-4e45-b850-c912ddd2d0bf --name my-new-route-name` command.

The output looks as follows:

```text
Id           6bb85c44-7187-4e45-b850-c912ddd2d0bf
Name         my-new-route-name
Rules
             action              send
             inclusion_filters
                                 operand    location
                                 operator   in
                                 values     [us-south, eu-de]

             targets
                                 id            0b4e6aa9-257c-4a3a-ae42-9a36a5b3adc7
                                 name          cl-target2
                                 target_type   cloud-logs

                                 id            6f9137b3-2bb9-4724-851b-153e23c82d80
                                 name          cl-platform-logs-sydney-113
                                 target_type   cloud-logs


             action              send
             inclusion_filters
                                 operand    location
                                 operator   is
                                 values     [us-east]

             targets
                                 id            0b4e6aa9-257c-4a3a-ae42-9a36a5b3adc7
                                 name          cl-target2
                                 target_type   cloud-logs

                                 id            6f9137b3-2bb9-4724-851b-153e23c82d80
                                 name          cl-platform-logs-sydney-113
                                 target_type   cloud-logs


Created At   2026-04-29T13:01:11.685Z
Updated At   2026-04-29T13:07:32.677Z
Managed By   account

```
{: screen}


## Deleting a route using the CLI
{: #route-manage-cli-delete}
{: cli}

Use this command to delete a route.

```sh
ibmcloud logs-router route delete --id route [--force]
```
{: pre}

### Command options
{: #route-manage-cli-delete-cmd}

`--id ROUTE`
:   The ID of the route.

`--force` | `-f`
:   Will delete the route without providing the user with any additional prompt.

`help` | `--help` | `-h`
:   List options available for the command.

### Example
{: #route-manage-cli-delete-sample}

The following is an example using the `ibmcloud logs-router route delete --id 6bb85c44-7187-4e45-b850-c912ddd2d0bf` command.

You are asked to confirm deletion of the route.  The following is the output of running the delete command:

```text
Are you sure you want to delete? [y/n]> y
OK
```
{: screen}


## Getting information about a route using the CLI
{: #route-manage-cli-read}
{: cli}

Use this command to get information about a route for an {{site.data.keyword.logs_routing_full_notm}} region.

```sh
ibmcloud logs-router route get --id route [--output FORMAT]
```
{: pre}

### Command options
{: #route-manage-cli-read-cmd}

`--route ROUTE`
:   The ID of the route.

`--output FORMAT`
:   Currently supported format is JSON. If specified, output will be returned in JSON format. If `JSON` is not specified, output will be returned in a tabular format.

`help` | `--help` | `-h`
:   List options available for the command.

### Example
{: #route-manage-cli-read-sample}

The following is an example using the `ibmcloud logs-router route get --id 2a4739ee-7cd6-4e05-b60d-9b2e7b0b7981` command showing a route.

The output looks as follows:

```text
Id           2a4739ee-7cd6-4e05-b60d-9b2e7b0b7981
Name         route2
Rules
             action    send
             targets
                       id            0b4e6aa9-257c-4a3a-ae42-9a36a5b3adc7
                       name          cl-target2
                       target_type   cloud-logs

                       id            6f9137b3-2bb9-4724-851b-153e23c82d80
                       name          cl-platform-logs-sydney-113
                       target_type   cloud-logs


Created At   2026-04-29T12:55:23.106Z
Updated At   2026-04-29T12:55:23.106Z
Managed By   account
```
{: screen}

## Listing all routes in a region
{: #route-manage-cli-list}
{: cli}

Use this command to list the configured routes for an {{site.data.keyword.logs_routing_full_notm}} region.

```sh
ibmcloud logs-router route list [--output FORMAT]
```
{: pre}

### Command options
{: #route-manage-cli-list-cmd}

`--output FORMAT`
:   Currently supported format is JSON. If specified, output will be returned in JSON format.  If `JSON` is not specified, output will be returned in a tabular format.

`help` | `--help` | `-h`
:   List options available for the command.

### Example
{: #route-manage-cli-list-sample}

The following is an example using the `ibmcloud logs-router route list` command.



## API routes and actions
{: #route-manage-api-actions}
{: api}

The following table lists the actions that you can run to manage routes:

| Action                     | REST API Method  | API_URL                                          |
|----------------------------|------------------|--------------------------------------------------|
| Create a route            | `POST`            | `<ENDPOINT>/v3/routes`              |
| Update a route            | `PATCH`           | `<ENDPOINT>/v3/routes/<route_ID>`  |
| Delete a route            | `DELETE`          | `<ENDPOINT>/v3/routes/<route_ID>`  |
| Read a route              | `GET`             | `<ENDPOINT>/v3/routes/<route_ID>`  |
| List all routes           | `GET`             | `<ENDPOINT>/v3/routes`             |
{: caption="route actions by using the {{site.data.keyword.logs_routing_full_notm}} REST API" caption-side="top"}

You can use private and public endpoints to manage routes. For more information about the list of `ENDPOINTS` that are available, see [Endpoints](/docs/logs-router?topic=logs-router-endpoints).
* You can manage routes from the private network using an API endpoint with the following format: `https://api.private.REGION.logs-router.cloud.ibm.com`
* You can manage routes from the public network using an API endpoint with the following format: `https://api.REGION.logs-router.cloud.ibm.com`

You can disable the public endpoints by updating the account settings. For more information, see [Configuring route and region settings](/docs/logs-router?topic=logs-router-endpoints).
{: note}

For more information about the REST API, see [routes](https://{DomainName}/apidocs/logs-router/logs-router-v3#create-route){: external}.



## API prerequisites
{: #route-manage-api-prereqs}
{: api}

To make API calls to manage routes, complete the following steps:
1. Get an IAM access token. For more information, see [Retrieving IAM access tokens](/docs/logs-router?topic=logs-router-retrieve-access-token&interface=api).
2. Identify the API endpoint in the region where you plan to configure or manage a route. For more information, see [Endpoints](/docs/logs-router?topic=logs-router-endpoints).

## Creating a route using the API
{: #route-manage-api-create}
{: api}

You can use the following cURL command to create a route:

Route names are unique in the account. You cannot reuse a route name to configure multiple destinations.
{: note}

```shell
curl -X POST <ENDPOINT>/v3/routes -H "Authorization: $ACCESS_TOKEN" -H "content-type: application/json" -d '{
    "name": "ROUTE_NAME",
    "rules": [
      {
        "action": "send",
        "targets": [
          {
            "id": "50375218-7cff-4234-bbb4-171bebab8408"
          },
          {
            "id": "c7519d8a-5f97-498b-a229-8542f60955cd"
          }
        ],
        "inclusion_filters": [
          {
            "operand": "location",
            "operator": "is",
            "values": ["us-east"]
          }
        ]
      }
    ]
  }'
```
{: codeblock}

Where

`<ENDPOINT>`
:   API endpoint in the region where you plan to configure or manage a route. For more information, see [Endpoints](/docs/logs-router?topic=logs-router-endpoints).

`ROUTE_NAME`
:   The name to be given to the route.

    The name to be given to the route.

    The maximum length is 1000 characters.

    The minimum length is 1 character.

    The name cannot include any special characters other than empty space, `-`, `.`, `_`, `:`.

    Do not include any personal identifying information (PII) in any resource names.
    {: important}

`RULES`
:   JSON string or a path to a JSON file prefix with `@` that define how platform logs are routed.

    Rules are evaluated in the order in which they are configured.

    The maximum length is 10 rules.

    For more information, see [Defining routing rules](/docs/logs-router?topic=logs-router-route_rules_definitions).


For example, you can use the following cURL request to create a route:

```shell
curl -X POST https://api.us-south.logs-router.cloud.ibm.com/v3/routes -H "Authorization: $ACCESS_TOKEN" -H "content-type: application/json" -d '{
    "name": "My-route",
    "rules": [
      {
        "action": "send",
        "targets": [
          {
            "id": "0b4e6aa9-257c-4a3a-ae42-9a36a5b3adc7"
          },
          {
            "id": "6f9137b3-2bb9-4724-851b-153e23c82d80"
          }
        ],
        "inclusion_filters": [
          {
            "operand": "location",
            "operator": "is",
            "values": ["us-east"]
          },
          {
            "operand": "service_name",
            "operator": "in",
            "values": ["codeengine","container-registry"]
          }
        ]
      }
    ]
  }'
```
{: screen}

The output looks as follows:

```text
{"id":"c7673f63-1260-4f86-bc1a-aaedb89391d8","name":"route5","crn":"crn:v1:bluemix:public:logs-router:global:a/xxxxxxx::route:c7673f63-1260-4f86-bc1a-aaedb89391d8","rules":[{"action":"send","targets":[{"id":"0b4e6aa9-257c-4a3a-ae42-9a36a5b3adc7","name":"cl-target2","crn":"crn:v1:bluemix:public:logs-router:eu-gb:a/xxxxxxx::target:0b4e6aa9-257c-4a3a-ae42-9a36a5b3adc7","target_type":"cloud-logs"},{"id":"6f9137b3-2bb9-4724-851b-153e23c82d80","name":"cl-platform-logs-sydney-113","crn":"crn:v1:bluemix:public:logs-router:eu-gb:a/xxxxxxx::target:6f9137b3-2bb9-4724-851b-153e23c82d80","target_type":"cloud-logs"}],"inclusion_filters":[{"operand":"location","operator":"is","values":["us-east"]}]}],"managed_by":"account","created_at":"2026-04-29T13:26:34.237Z","updated_at":"2026-04-29T13:26:34.237Z"}
```
{: screen}

## Updating a route using the API
{: #route-manage-api-update}
{: api}

You can modify the name of a route and rules. Any specified value that is different from when the route was originally created will be updated to the value specified in the request.

When you update a route, you must include the route information in the data section of the request.
- You must pass all fields.
- Update the fields that need to be changed.

Route names are unique in the account. You cannot reuse a route name to configure multiple destinations.
{: note}

You can use the following cURL command to update a route:

```shell
curl -X PATCH <ENDPOINT>/v3/routes/ROUTE_ID -H "Authorization: $ACCESS_TOKEN" -H "content-type: application/json" -d '{
    "name": "ROUTE_NAME",
    "rules": [
      {
        "action": "send",
        "targets": [
          {
            "id": "d3ebcda8-953d-45ea-a4a7-b47f8c4a9742"
          }
        ],
        "inclusion_filters": [
          {
            "operand": "location",
            "operator": "is",
            "values": ["eu-de"]
          }
        ]
      }
    ]
  }'
```
{: codeblock}

Where

`<ENDPOINT>`
:   API endpoint in the region where you plan to configure or manage a route. For more information, see [Endpoints](/docs/logs-router?topic=logs-router-endpoints).

`ROUTE_ID`
:   ID of the route.

`ROUTE_NAME`
:   The name to be given to the route.

    The name to be given to the route.

    The maximum length is 1000 characters.

    The minimum length is 1 character.

    The name cannot include any special characters other than empty space, `-`, `.`, `_`, `:`.

    Do not include any personal identifying information (PII) in any resource names.
    {: important}

`RULES`
:   JSON string or a path to a JSON file prefix with `@` that define how platform logs are routed.

    Rules are evaluated in the order in which they are configured.

    The maximum length is 10 rules.

    For more information, see [Defining routing rules](/docs/logs-router?topic=logs-router-route_rules_definitions).


For example, you can use the following cURL request to create a route in Dallas:

```shell
curl -X PATCH https://api.us-south.logs-router.cloud.ibm.com/v3/routes/c7673f63-1260-4f86-bc1a-aaedb89391d8 -H "Authorization: $ACCESS_TOKEN" -H "content-type: application/json" -d '{
    "name": "My new route name",
    "rules": [
      {
        "action": "send",
        "targets": [
          {
            "id": "0b4e6aa9-257c-4a3a-ae42-9a36a5b3adc7"
          },
          {
            "id": "6f9137b3-2bb9-4724-851b-153e23c82d80"
          }
        ],
        "inclusion_filters": [
          {
            "operand": "location",
            "operator": "is",
            "values": ["us-east"]
          }
        ]
      }
    ]
    }
  }'
```
{: screen}


## Deleting a route using the API
{: #route-manage-api-delete}
{: api}

You can use the following cURL command to delete a route:

```shell
curl -X DELETE <ENDPOINT>/v3/routes/<route_ID> -H "Authorization: $ACCESS_TOKEN" -H "content-type: application/json"
```
{: codeblock}

Where

`<ENDPOINT>`
:   API endpoint in the region where you plan to configure or manage a route. For more information, see [Endpoints](/docs/logs-router?topic=logs-router-endpoints).

`<route_ID>`
:   ID of the route.

For example, you can use the following cURL request to delete a route with the ID `c7673f63-1260-4f86-bc1a-aaedb89391d8`:

```shell
curl -X DELETE https://api.us-south.logs-router.cloud.ibm.com/v3/routes/c7673f63-1260-4f86-bc1a-aaedb89391d8 -H "Authorization: $ACCESS_TOKEN" -H "content-type: application/json"
```
{: screen}


## Viewing a route using the API
{: #route-manage-api-read}
{: api}

You can use the following cURL command to view the configuration details of 1 route:

```shell
curl -X GET <ENDPOINT>/v3/routes/<route_ID> -H "Authorization: $ACCESS_TOKEN" -H "content-type: application/json"
```
{: codeblock}

Where

`<ENDPOINT>`
:   API endpoint in the region where you plan to configure or manage a route. For more information, see [Endpoints](/docs/logs-router?topic=logs-router-endpoints).

`<route_ID>`
:   ID of the route.

For example, you can run the following cURL request to get information about a route with the ID `00000000-0000-0000-0000-000000000000`:

```shell
curl -X GET https://api.us-south.logs-router.cloud.ibm.com/v3/routes/c7673f63-1260-4f86-bc1a-aaedb89391d8 -H "Authorization: $ACCESS_TOKEN" -H "content-type: application/json"
```
{: screen}



## Listing all routes using the API
{: #route-manage-api-list}
{: api}

You can use the following cURL command to view all routes:

```shell
curl -X GET <ENDPOINT>/v3/routes -H "Authorization: $ACCESS_TOKEN" -H "content-type: application/json"
```
{: codeblock}

Where

- `<ENDPOINT>` is the API endpoint in the region where you plan to configure or manage a route. For more information, see [Endpoints](/docs/logs-router?topic=logs-router-endpoints).


For example, you can run the following cURL request to get information about the routes that are defined in Dallas:

```shell
curl -X GET https://api.us-south.logs-router.cloud.ibm.com/v3/routes -H "Authorization: $ACCESS_TOKEN" -H "content-type: application/json"
```
{: screen}




## HTTP response codes
{: #route-manage-api-rc}
{: api}

When you use the {{site.data.keyword.logs_routing_full_notm}} REST API, you can get standard HTTP response codes to indicate whether a method completed successfully.

- A 200 response always indicates success.
- A 4xx response indicates a failure.
- A 5xx response indicates an internal system error.

See the following table for some HTTP response codes:

| Status code | Status | Description |
|-------------|--------|-------------|
| `200` |	OK | The request was successful. |
| `201` |	OK | The request was successful. A resource is created. |
| `400` | Bad Request |	The request was unsuccessful. You might be missing a parameter that is required. |
| `401` |	Unauthorized | The IAM token that is used in the API request is invalid or expired. |
| `403` |	Forbidden | The operation is forbidden due to insufficient permissions. |
| `404` | Not Found |	The requested resource doesn't exist or is already deleted. |
| `409` | Conflict | There is a conflict with the request data and the state of resources in system. |
| `429` |	Too Many Requests |	Too many requests hit the API too quickly. |
| `500` |	Internal Server Error |	Something went wrong in {{site.data.keyword.logs_routing_full_notm}} processing. |
{: caption="List of HTTP response codes" caption-side="top"}
