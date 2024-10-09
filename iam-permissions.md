---

copyright:
  years:  2023, 2024
lastupdated: "2024-10-09"

keywords:

subcollection: logs-router

---

{{site.data.keyword.attribute-definition-list}}

# Granting IAM permissions
{: #iam-permissions}

To manage the {{site.data.keyword.logs_routing_full_notm}} service in an account so that you can configure collection and routing of platform logs that are generated in the account, you must have the `Manager` role for {{site.data.keyword.logs_routing_full_notm}}. To see what IAM roles are available for {{site.data.keyword.logs_routing_full_notm}}, see [Managing IAM access](/docs/logs-router?topic=logs-router-iam).
{: shortdesc}

If you are the account owner, you might already have sufficient access without requiring additional permissions.
{: tip}

If you have the IAM permission to create policies and authorizations, you can grant only the level of access that you have as a user of the target service. For example, if you have viewer access for the target service, you can assign only the viewer role for the authorization. If you attempt to assign a higher permission such as administrator, it might appear that permission is granted, however, only the highest level permission you have for the target service, that is viewer, will be assigned. 
{: important}

## Assigning access to {{site.data.keyword.logs_routing_full_notm}} in the console
{: #tenant-iam-permissions-ui}
{: ui}


There are two common ways to assign access to {{site.data.keyword.logs_routing_full_notm}} in the console:

* Access groups. You can manage access groups and their access from the **Manage** > **Access (IAM)** > **Access groups** page in the console. For more information, see [Assigning access to a group in the console](/docs/account?topic=account-groups&interface=ui#access_ag).

* Access policies per user. You can manage access policies per user from the **Manage** > **Access (IAM)** > **Users** page in the console. For information about the steps to assign IAM access, see [Managing access to resources](https://cloud.ibm.com/docs/account?topic=account-assign-access-resources&interface=ui#access-resources-console).




## Setting up permissions by using the CLI
{: #tenant-iam-permissions-cli}
{: cli}

Granting the role can be done by using the `ibmcloud` CLI.

Use the appropriate command for the type of identity:

| Type of identity  | Command |
|-------------------|---------|
| User account      | `ibmcloud iam user-policy-create <username> --roles Manager --service-name logs-router` |
| Service ID        | `ibmcloud iam service-policy-create <serviceID> --roles Manager --service-name logs-router` |
| Trusted profile   | `ibmcloud iam tp-policy-create <trustedProfile> --roles Manager --service-name logs-router` |
| Access group      | `ibmcloud iam access-group-policy-create ACCESS_GROUP --roles Manager --service-name logs-router` |
{: caption="Command to grant IAM permissions by type of identity" caption-side="top"}

Instead of assigning roles directly to identities, a common strategy is to assign roles to access groups, and add identities as members to those access groups. For more information about access groups, see [setting up access groups.](/docs/account?topic=account-groups&interface=cli)
{: tip}


For step-by-step instructions for assigning, removing, and reviewing access, see [Assigning access to resources by using the CLI](/docs/account?topic=account-assign-access-resources&interface=cli#access-resources-cli).



## Assigning access by using the API
{: #tenant-iam-permissions-api}
{: api}


For step-by-step instructions for assigning, removing, and reviewing access, see [Assigning access to resources by using the API](/docs/account?topic=account-assign-access-resources&interface=api) or the [Create a policy API docs](/apidocs/iam-policy-management#create-policy). Role cloud resource names (CRN) in the following table are used to assign access with the API.


| Role name | Role CRN |
|---------------|-----------------|
| Viewer                 | `crn:v1:bluemix:public:logs-router::::serviceRole:Viewer`        |
| Operator               | `crn:v1:bluemix:public:logs-router::::serviceRole:Operator`      |
| Editor                 | `crn:v1:bluemix:public:logs-router::::serviceRole:Editor`        |
| Administrator          | `crn:v1:bluemix:public:logs-router::::serviceRole:Administrator` |
{: caption="Role ID values for API use" caption-side="bottom"}

The following example is for assigning the `Viewer` role for {{site.data.keyword.logs_routing_full_notm}}:

Use `logs` for the service name, and refer to the Role ID values table to ensure that you're using the correct value for the CRN.
{: tip}

```curl
curl -X POST 'https://iam.cloud.ibm.com/v1/policies' -H 'Authorization: Bearer $TOKEN' -H 'Content-Type: application/json' -d '{
  "type": "access",
  "description": "Viewer role for IBM Cloud Logs Routing",
  "subjects": [
    {
      "attributes": [
        {
          "name": "iam_id",
          "value": "IBMid-123453user"
        }
      ]
    }
  ],
  "roles":[
    {
      "role_id": "crn:v1:bluemix:public:logs-router::::serviceRole:Viewer"
    }
  ],
  "resources":[
    {
      "attributes": [
        {
          "name": "accountId",
          "value": "$ACCOUNT_ID"
        },
        {
          "name": "serviceName",
          "value": "logs-router"
        }
      ]
    }
  ]
}'
```
{: curl}
{: codeblock}

```java
SubjectAttribute subjectAttribute = new SubjectAttribute.Builder()
      .name("iam_id")
      .value("IBMid-123453user")
      .build();

PolicySubject policySubjects = new PolicySubject.Builder()
      .addAttributes(subjectAttribute)
      .build();

PolicyRole policyRoles = new PolicyRole.Builder()
      .roleId("crn:v1:bluemix:public:logs-router::::serviceRole:Viewer")
      .build();

ResourceAttribute accountIdResourceAttribute = new ResourceAttribute.Builder()
      .name("accountId")
      .value("ACCOUNT_ID")
      .operator("stringEquals")
      .build();

ResourceAttribute serviceNameResourceAttribute = new ResourceAttribute.Builder()
      .name("serviceName")
      .value("logs")
      .operator("stringEquals")
      .build();

PolicyResource policyResources = new PolicyResource.Builder()
      .addAttributes(accountIdResourceAttribute)
      .addAttributes(serviceNameResourceAttribute)
      .build();

CreatePolicyOptions options = new CreatePolicyOptions.Builder()
      .type("access")
      .subjects(Arrays.asList(policySubjects))
      .roles(Arrays.asList(policyRoles))
      .resources(Arrays.asList(policyResources))
      .build();

Response<Policy> response = service.createPolicy(options).execute();
Policy policy = response.getResult();

System.out.println(policy);
```
{: java}
{: codeblock}

```javascript
const policySubjects = [
  {
    attributes: [
      {
        name: 'iam_id',
        value: 'IBMid-123453user',
      },
    ],
  },
];
const policyRoles = [
  {
    role_id: 'crn:v1:bluemix:public:logs-router::::serviceRole:Viewer',
  },
];
const accountIdResourceAttribute = {
  name: 'accountId',
  value: 'ACCOUNT_ID',
  operator: 'stringEquals',
};
const serviceNameResourceAttribute = {
  name: 'serviceName',
  value: 'logs-router',
  operator: 'stringEquals',
};
const policyResources = [
  {
    attributes: [accountIdResourceAttribute, serviceNameResourceAttribute]
  },
];
const params = {
  type: 'access',
  subjects: policySubjects,
  roles: policyRoles,
  resources: policyResources,
};

iamPolicyManagementService.createPolicy(params)
  .then(res => {
    examplePolicyId = res.result.id;
    console.log(JSON.stringify(res.result, null, 2));
  })
  .catch(err => {
    console.warn(err)
  });
```
{: javascript}
{: codeblock}

```python
policy_subjects = PolicySubject(
  attributes=[SubjectAttribute(name='iam_id', value='IBMid-123453user')])
policy_roles = PolicyRole(
  role_id='crn:v1:bluemix:public:logs-router::::serviceRole:Viewer')
account_id_resource_attribute = ResourceAttribute(
  name='accountId', value='ACCOUNT_ID')
service_name_resource_attribute = ResourceAttribute(
  name='serviceName', value='logs-router')
policy_resources = PolicyResource(
  attributes=[account_id_resource_attribute,
        service_name_resource_attribute])

policy = iam_policy_management_service.create_policy(
  type='access',
  subjects=[policy_subjects],
  roles=[policy_roles],
  resources=[policy_resources]
).get_result()

print(json.dumps(policy, indent=2))
```
{: python}
{: codeblock}

```go
subjectAttribute := &iampolicymanagementv1.SubjectAttribute{
  Name:  core.StringPtr("iam_id"),
  Value: core.StringPtr("IBMid-123453user"),
}
policySubjects := &iampolicymanagementv1.PolicySubject{
  Attributes: []iampolicymanagementv1.SubjectAttribute{*subjectAttribute},
}
policyRoles := &iampolicymanagementv1.PolicyRole{
  RoleID: core.StringPtr("crn:v1:bluemix:public:logs-router::::serviceRole:Viewer"),
}
accountIDResourceAttribute := &iampolicymanagementv1.ResourceAttribute{
  Name:     core.StringPtr("accountId"),
  Value:    core.StringPtr("ACCOUNT_ID"),
  Operator: core.StringPtr("stringEquals"),
}
serviceNameResourceAttribute := &iampolicymanagementv1.ResourceAttribute{
  Name:     core.StringPtr("serviceName"),
  Value:    core.StringPtr("logs-router"),
  Operator: core.StringPtr("stringEquals"),
}
policyResources := &iampolicymanagementv1.PolicyResource{
  Attributes: []iampolicymanagementv1.ResourceAttribute{
    *accountIDResourceAttribute, *serviceNameResourceAttribute}
}

options := iamPolicyManagementService.NewCreatePolicyOptions(
  "access",
  []iampolicymanagementv1.PolicySubject{*policySubjects},
  []iampolicymanagementv1.PolicyRole{*policyRoles},
  []iampolicymanagementv1.PolicyResource{*policyResources},
)

policy, response, err := iamPolicyManagementService.CreatePolicy(options)
if err != nil {
  panic(err)
}
b, _ := json.MarshalIndent(policy, "", "  ")
fmt.Println(string(b))
```
{: go}
{: codeblock}

## Assigning access by using Terraform
{: #iam-assign-access-terraform}
{: terraform}

The following example is for assigning the `Viewer` role for {{site.data.keyword.logs_routing_full_notm}}:

Use `logs-router` for the service name.
{: note}

```terraform
resource "ibm_iam_user_policy" "policy" {
  ibm_id = "test@example.com"
  roles  = ["Viewer"]

  resources {
    service = "logs-router"
  }
}
```
{: codeblock}

For more information, see the terraform resource [ibm_iam_user_policy](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/iam_user_policy){: external}.
