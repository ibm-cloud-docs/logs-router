---
copyright:
  years:  2023, 2025
lastupdated: "2025-02-23"

keywords:

subcollection: logs-router

---

{{site.data.keyword.attribute-definition-list}}

# Setting up Terraform for {{site.data.keyword.logs_routing_full_notm}}
{: #terraform-setup}

Terraform on {{site.data.keyword.cloud}} enables predictable and consistent creation of {{site.data.keyword.cloud_notm}} services so that you can rapidly build complex, multi-tier cloud environments following Infrastructure as Code (IaC) principles. Similar to using the {{site.data.keyword.cloud_notm}} CLI or API and SDKs, you can automate the creation, update, and deletion of your {{site.data.keyword.logs_routing_full_notm}} tenants and targets by using HashiCorp Configuration Language (HCL).
{: shortdesc}


Looking for a managed Terraform on {{site.data.keyword.cloud}} solution? Try out [{{site.data.keyword.bplong}}](/docs/schematics?topic=schematics-getting-started). With {{site.data.keyword.bpshort}}, you can use the Terraform scripting language that you are familiar with, but you don't have to worry about setting up and maintaining the Terraform command line and the {{site.data.keyword.cloud}} Provider plug-in. {{site.data.keyword.bpshort}} also provides pre-defined Terraform templates that you can easily install from the {{site.data.keyword.cloud}} catalog.
{: tip}

## Installing Terraform and configuring resources for {{site.data.keyword.logs_routing_full_notm}}
{: #terraform-install-cli}

Before you can create workloads with Terraform, make sure that you have completed the following prerequisites.

- Make sure that you have set up permissions to manage tenants in the account. For more information, see [Setting up IAM permissions for managing tenants](/docs/logs-router?topic=logs-router-iam&interface=ui).
- Install the Terraform CLI and configure the {{site.data.keyword.cloud_notm}} Provider plug-in for Terraform. For more information, see the tutorial for [Getting started with Terraform on {{site.data.keyword.cloud}}](/docs/ibm-cloud-provider-for-terraform?topic=ibm-cloud-provider-for-terraform-getting-started). The plug-in abstracts the {{site.data.keyword.cloud_notm}} APIs that are used to complete this task.
- Create a Terraform configuration file that is named `main.tf`. In this file, you define resources by using HashiCorp Configuration Language. For more information, see the [Terraform documentation](https://developer.hashicorp.com/terraform/language){: external}.

1. Create a {{site.data.keyword.logs_routing_full_notm}} tenant by using the `ibm_logs_router_tenant` resource argument in your `main.tf` file.

   ```terraform
   terraform {
      required_version = ">= 0.15"
      required_providers {
        ibm = {
            source = "ibm-cloud/ibm"
            version = ">=1.69.2"
        }
      }
   }
   provider "ibm" {
      ibmcloud_api_key = var.ibmcloud_api_key
      region = "us-south"
   }

   resource "ibm_logs_router_tenant" "logs_router_tenant_instance" {
      name = "cloud-logs-router-tenant"
      region = "eu-de"
      targets {
        log_sink_crn = "crn:v1:bluemix:public:logs:eu-de:a/1246b8fa0a174a71699f3affa2f18d78:3517d2ed-9429-af34-ad32-34248395cbc8::"
        name = "my-cloud-logs-target"
        parameters {
            host = "3517d2ed-9429-af34-ad32-34248395cbc8.ingress.eu-de.logs.cloud.ibm.com"
            port = 443
        }
      }
   }
   ```
   {: codeblock}


2. After you finish building your configuration file, initialize the Terraform CLI. For more information, see [Initializing Working Directories](https://developer.hashicorp.com/terraform/cli/init){: external}.

   ```terraform
   terraform init
   ```
   {: pre}


3. Create the tenant from the `main.tf` file. For more information, see [Provisioning Infrastructure with Terraform](https://developer.hashicorp.com/terraform/cli/run){: external}.

   1. Run `terraform plan` to generate a Terraform execution plan to preview the proposed actions.

      ```terraform
      terraform plan
      ```
      {: pre}


      ```terraform
        Terraform used the selected providers to generate the following execution plan. Resource actions are indicated with the following symbols:
        + create

        Terraform will perform the following actions:

        # ibm_logs_router_tenant.logs_router_tenant_instance will be created
        + resource "ibm_logs_router_tenant" "logs_router_tenant_instance" {
            + created_at = (known after apply)
            + crn        = (known after apply)
            + etag       = (known after apply)
            + id         = (known after apply)
            + name       = "cloud-logs-router-tenant"
            + region     = "eu-de"
            + updated_at = (known after apply)

            + targets {
                + created_at   = (known after apply)
                + etag         = (known after apply)
                + id           = (known after apply)
                + log_sink_crn = "crn:v1:bluemix:public:logs:eu-de:a/1246b8fa0a174a71699f3affa2f18d78:3517d2ed-9429-af34-ad32-34248395cbc8::"
                + name         = "my-cloud-logs-target"
                + type         = (known after apply)
                + updated_at   = (known after apply)

                + parameters {
                    + host = "3517d2ed-9429-af34-ad32-34248395cbc8.ingress.eu-de.logs.cloud.ibm.com"
                    + port = 443
                    }
                }
            }

        Plan: 1 to add, 0 to change, 0 to destroy.      
      ```
      {: screen}

   2. Run `terraform apply` to create the resources that are defined in the plan.

      ```terraform
      terraform apply
      ```
      {: pre}

      ```text
        Terraform used the selected providers to generate the following execution plan. Resource actions are indicated with the following symbols:
        + create

        Terraform will perform the following actions:

        # ibm_logs_router_tenant.logs_router_tenant_instance will be created
        + resource "ibm_logs_router_tenant" "logs_router_tenant_instance" {
            + created_at = (known after apply)
            + crn        = (known after apply)
            + etag       = (known after apply)
            + id         = (known after apply)
            + name       = "cloud-logs-router-tenant"
            + region     = "eu-de"
            + updated_at = (known after apply)

            + targets {
                + created_at   = (known after apply)
                + etag         = (known after apply)
                + id           = (known after apply)
                + log_sink_crn = "crn:v1:bluemix:public:logs:eu-de:a/1246b8fa0a174a71699f3affa2f18d78:3517d2ed-9429-af34-ad32-34248395cbc8::"
                + name         = "my-cloud-logs-target"
                + type         = (known after apply)
                + updated_at   = (known after apply)

                + parameters {
                    + host = "3517d2ed-9429-af34-ad32-34248395cbc8.ingress.eu-de.logs.cloud.ibm.com"
                    + port = 443
                    }
                }
            }

        Plan: 1 to add, 0 to change, 0 to destroy.

        Do you want to perform these actions?
        Terraform will perform the actions described above.
        Only 'yes' will be accepted to approve.

        Enter a value: yes

        ibm_logs_router_tenant.logs_router_tenant_instance: Creating...
        ibm_logs_router_tenant.logs_router_tenant_instance: Creation complete after 3s [id=89b89296-3091-4687-b656-4d02abfce0a5]

        Apply complete! Resources: 1 added, 0 changed, 0 destroyed.
      ```
      {: screen}

4. You can see a list the tenants that are configured in the account in the [{{site.data.keyword.logs_routing_full_notm}} console](/docs/logs-router?topic=logs-router-tenants-list#tenants-list-ui). 

## What's next?
{: #terraform-setup-next}

Now that you successfully created your first {{site.data.keyword.logs_routing_full_notm}} tenant with Terraform on {{site.data.keyword.cloud_notm}}, you can create additional targets for this tenant or create new tenants in different regions.

### Resources 
{: #terraform-supported-resources}

* [`ibm_logs_router_tenant`](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/logs-router_tenant){: external}

### Data Sources
{: #terraform-supported-data-sources}

* [`ibm_logs_router_targets`](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/data-sources/logs-router_targets){: external}
* [`ibm_logs_router_tenants`](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/data-sources/logs-router_tenants){: external}

For more information about {{site.data.keyword.cloud_notm}} and Terraform, see [IBM Cloud Provider](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs){: external}.
