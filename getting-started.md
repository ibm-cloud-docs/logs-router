---

copyright:
  years:  2023, 2024
lastupdated: "2024-08-24"

keywords:

subcollection: logs-router

---

{{site.data.keyword.attribute-definition-list}}

# Getting started with {{site.data.keyword.logs_routing_full}}
{: #getting-started}

Use the {{site.data.keyword.logs_routing_full_notm}} service to route logs from your {{site.data.keyword.cloud_notm}} account to your chosen target. You can route logs from your own {{site.data.keyword.cloud_notm}} workloads, such as, applications on your [{{site.data.keyword.containerlong_notm}}](/docs/containers) or [{{site.data.keyword.openshiftlong_notm}}](/docs/openshift) clusters, and from selected {{site.data.keyword.cloud_notm}} service instances.
{: shortdesc}

![Flow of routed logs](../images/get-started.png "Flow of routed logs"){: caption="Figure 1. Flow of routed logs" caption-side="bottom"}


Complete the following steps to start using {{site.data.keyword.logs_routing_full}}:

## Before you begin
{: #getting-started-before-you-begin}
{: step}

### Learn about {{site.data.keyword.logs_routing_full_notm}}
{: #getting-started-before-you-begin-0}

[Learn more about {{site.data.keyword.logs_routing_full_notm}}](/docs/logs-router?topic=logs-router-about).

To use the {{site.data.keyword.logs_routing_full}} service, consider the following information:
- You must create a tenant in a region in your account that defines the target destination of logs collected in that region and the rules on how they are routed.

    You must create (onboard) an {{site.data.keyword.logs_routing_full_notm}} tenant in your {{site.data.keyword.cloud_notm}} account in at least one [supported {{site.data.keyword.cloud}} region](/docs/logs-router?topic=logs-router-locations).

- You must install the {{site.data.keyword.logs_routing_full_notm}} agent on 1 or more log sources. You must configure the agent to send log events to the {{site.data.keyword.logs_routing_full_notm}} service in your account.

- If you are connecting log sources through an {{site.data.keyword.vpc_short}}, you must configure an ingestion virtual private endpoint (VPE) to privately connect agents running on those log sources to the {{site.data.keyword.logs_routing_full_notm}} service.

### {{site.data.keyword.cloud_notm}} prerequisites
{: #getting-started-before-you-begin-1}

1. [Check the regions where the {{site.data.keyword.logs_routing_full_notm}} service is available](/docs/logs-router?topic=logs-router-locations).

2. If you don't have an {{site.data.keyword.cloud_notm}} account, [register an {{site.data.keyword.cloud_notm}} account](https://cloud.ibm.com/login){: external}. You need an IBMid to work in {{site.data.keyword.cloud_notm}}.

3. Check that the user who is configuring {{site.data.keyword.logs_routing_full_notm}} for the {{site.data.keyword.cloud}} account has sufficient permissions to manage the {{site.data.keyword.logs_routing_full_notm}} service. For more information, see [Managing IAM access for {{site.data.keyword.logs_routing_full_notm}}](/docs/logs-router?topic=logs-router-iam).

4. If you do not have a cluster, [create a cluster](/docs/containers?topic=containers-clusters) or use an existing {{site.data.keyword.containerlong_notm}} cluster.


### Install prerequisites
{: #getting-started-before-you-begin-2}

Install the following tools:
- [Download and install the {{site.data.keyword.cloud}} CLI](/docs/cli).
- If you are installing the agent on a Kubernetes cluster, [install the Kubernetes CLI (`kubectl`)](https://kubernetes.io/docs/tasks/tools/){: external} .
- If you are installing the agent on an [{{site.data.keyword.openshiftlong_notm}}](https://cloud.ibm.com/docs/openshift) cluster, [install the Red Hat Openshift CLI (`oc`)](/docs/openshift?topic=openshift-cli-install).
- Provision an {{site.data.keyword.logs_full_notm}} instance. For more information, see [Provisioning an instance](/docs/cloud-logs?topic=cloud-logs-instance-provision&interface=ui).
- If you are going to be running the agent in an {{site.data.keyword.vpc_full}} and will be using the {{site.data.keyword.cloud}} CLI to configure networking, [install the {{site.data.keyword.vpc_full}} CLI plugin](/docs/vpc?topic=vpc-set-up-environment&interface=cli).
- [Download and install jq](https://stedolan.github.io/jq/){: external} to process output and query desired results.
- [Download and install yq](https://github.com/mikefarah/yq?tab=readme-ov-file#install){: external} to read, write, and manipulate YAML files in a similar way to using `jq` for JSON files.

## Creating (onboarding) a tenant
{: #getting-started-onboard-tenant}
{: step}

{{site.data.content.tenant_definition-paragraph}}


### Enable connectivity
{: #getting-started-onboard-tenant-1}

You can manage {{site.data.keyword.logs_routing_full_notm}} by using the management API. The management API supports either a public endpoint or a private endpoint. A public endpoint can be reached over the internet, whereas a private endpoint can be accessed only from within the {{site.data.keyword.cloud_notm}} private network.

To create a tenant, you must use the {{site.data.keyword.logs_routing_full_notm}} management API.

Using the public endpoint does not require additional work in this step. Use the public endpoint to complete the steps in this tutorial.

For more information, see [Connecting to {{site.data.keyword.logs_routing_full_notm}}](/docs/logs-router?topic=logs-router-about#about_connecting).


### Collect information about the target destination
{: #getting-started-onboard-tenant-2}

{{site.data.keyword.logs_routing_full_notm}} supports the following targets:

- {{site.data.keyword.logs_full_notm}}
- {{site.data.keyword.la_full_notm}}


### Create a tenant in a region
{: #getting-started-onboard-tenant-3}

Create a {{site.data.keyword.logs_full_notm}} tenant by following [Creating an {{site.data.keyword.logs_full_notm}} tenant in {{site.data.keyword.logs_routing_full_notm}}](/docs/logs-router?topic=logs-router-onboard-cloud-logs-tenant).



## Installing the {{site.data.keyword.logs_routing_full_notm}} agent on your cluster
{: #getting-started-deploy-agent}
{: step}

The {{site.data.keyword.logs_routing_full_notm}} agent collects data from containerized workloads that run on a Kubernetes cluster.


### Enable connectivity
{: #getting-started-deploy-agent-1}

You can send logs to {{site.data.keyword.logs_routing_full_notm}} by using the ingestion API.

The ingestion API supports only private endpoints and is therefore not accessible from the public internet.
{: important}

The {{site.data.keyword.logs_routing_full}} supports the following types of endpoints to privately connect to {{site.data.keyword.logs_routing_full_notm}}:
- {{site.data.keyword.cloud_notm}} Cloud Service Endpoint (CSE)
- {{site.data.keyword.vpe_full}}.

Choose one of the following options to privately connect to {{site.data.keyword.logs_routing_full_notm}}:
- [Using virtual private endpoints for VPC to privately connect to {{site.data.keyword.logs_routing_full_notm}}](/docs/logs-router?topic=logs-router-vpe-connection).
- [Using service endpoints to privately connect to {{site.data.keyword.logs_routing_full_notm}}](/docs/logs-router?topic=logs-router-service-endpoints).


### Deploy the agent
{: #getting-started-deploy-agent-2}

- For {{site.data.keyword.containerlong_notm}} clusters:  Complete the steps in [Managing the {{site.data.keyword.logs_routing_full}} agent for {{site.data.keyword.containerlong_notm}} clusters](/docs/logs-router?topic=logs-router-agent-std-cluster&interface=api#agent-std-cluster-deploy).

- For {{site.data.keyword.openshiftlong_notm}} clusters:  Complete the steps in [Managing the {{site.data.keyword.logs_routing_full}} agent for {{site.data.keyword.openshiftlong_notm}} clusters](/docs/logs-router?topic=logs-router-agent-openshift&interface=api#agent-openshift-deploy).



## Verifying that logs are sent to the destination target
{: #getting-started-verify-logs}
{: step}

Verify that the logs for your cluster are routed to your {{site.data.keyword.logs_full_notm}} instance.

[Launch the {{site.data.keyword.logs_full_notm}} web UI](/docs/cloud-logs?topic=cloud-logs-instance-launch&interface=ui) for the target {{site.data.keyword.logs_full_notm}} instance and check that logs from your cluster are displayed.
