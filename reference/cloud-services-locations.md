---

copyright:
  years:  2023, 2024
lastupdated: "2024-08-16"

keywords:

subcollection: logs-router


---

{{site.data.keyword.attribute-definition-list}}


# {{site.data.keyword.cloud_notm}} services that generate platform logs
{: #cloud_services}

{{site.data.keyword.cloud}} services can send logs about their services to {{site.data.keyword.logs_routing_full_notm}}. These logs are called platform logs.
{: shortdesc}

The more information link for each service describes the logging support for that service.
{: tip}




## Compute serverless services
{: #serverless}

The following table lists services that send logs to {{site.data.keyword.logs_routing_full_notm}}:

| Service     | Description | More information |
|-------------|-------------|-------------------|
| {{site.data.keyword.satellitelong_notm}} | With {{site.data.keyword.satellitelong_notm}}, you can bring your own compute infrastructure to run {{site.data.keyword.cloud_notm}} services and consistently deploy, manage, and control your app workloads. | [More information](/docs/satellite?topic=satellite-health) |
{: caption="List of serverless compute services" caption-side="top"}


## Container services
{: #platform_container}

The following table lists services that send logs to {{site.data.keyword.logs_routing_full_notm}}:

| Service     | Description | More information |
|-------------|-------------|-------------------------------------------------------------------------|
| {{site.data.keyword.containerlong_notm}} | You can use the {{site.data.keyword.containerlong_notm}} service to deploy highly available apps in Docker containers that run in Kubernetes clusters. | [More information](/docs/containers?topic=containers-health) |
| {{site.data.keyword.openshiftlong_notm}} | With {{site.data.keyword.openshiftlong_notm}}, you can deploy apps on highly available clusters that come installed with the Red Hat OpenShift on IBM Cloud Container Platform software that is installed on Red Hat Enterprise Linux. | [More information](/docs/openshift?topic=openshift-health) |
| {{site.data.keyword.registryfull_notm}}  | You can use {{site.data.keyword.registrylong_notm}} to provide a multi-tenant private image registry that you can use to store and share your container images with users in your {{site.data.keyword.cloud_notm}} account. | [More information](/docs/Registry?topic=Registry-registry_logs) |
{: caption="List of container services" caption-side="top"}



## Database services
{: #database}

The following table lists services that send logs to {{site.data.keyword.logs_routing_full_notm}}:

| Service     | Description |  More information |
|-------------|-------------|--------------------------------------------------------------------------------------------|
| {{site.data.keyword.cloudant}} | {{site.data.keyword.cloudant_short_notm}} is a document-oriented database as a service (DBaaS). It stores data as documents in JSON format. | [More information](/docs/Cloudant?topic=Cloudant-log-analysis-integration) |
{: caption="List of database services" caption-side="top"}







## VPC services
{: #vpc}

The following table lists Cloud services that send logs to {{site.data.keyword.logs_routing_full_notm}}:


| Service          | Description | More information         |
|------------------|-------------|-------------------|
| [VPN](/docs/vpc?topic=vpc-using-vpn) | Use this service to connect private networks in a secure fashion. You can use VPN to set up an IPsec site-to-site tunnel between your VPC and your on-premise private network or another VPC. | [More info](/docs/vpc?topic=vpc-using-log-analysis-to-view-vpn-logs) |
| [Dedicated Host for VPC](/docs/vpc?topic=vpc-creating-dedicated-hosts-instances) | You can create a dedicated host to carve out a single-tenant compute node, free from users outside of your organization.  | [More information](/docs/vpc?topic=vpc-logging#logging-dedicated-host) |
| [Flow Log Collector](/docs/vpc?topic=vpc-flow-logs)| This service is used to collect and store information regarding the Internet Protocol (IP) traffic going to and from network interfaces within your Virtual Private Cloud (VPC) |[More information](/docs/vpc?topic=vpc-logging#logging-flow-log-collector_msgs) |
| [Snapshots for VPC](/docs/vpc?topic=vpc-snapshots-vpc-about)| This offering is used to create a point-in-time copy of your boot or data volume. |[More information](/docs/vpc?topic=vpc-logging&interface=ui#logging-snapshots) |
| [{{site.data.keyword.filestorage_vpc_short}}](/docs/vpc?topic=vpc-file-storage-vpc-about)| {{site.data.keyword.filestorage_vpc_short}} provides NFS-based file storage services within the VPC Infrastructure. |[More information](/docs/vpc?topic=vpc-logging&interface=ui#logging-file-share-replication) |
{: caption="List of IBM Cloud VPC services" caption-side="top"}

## Networking services
{: #networking}

The following table lists Cloud Networking services that send logs to {{site.data.keyword.logs_routing_full_notm}}:


| Service          | Description | More information         |
|------------------|-------------|-------------------|
| {{site.data.keyword.dns_full_notm}} | With DNS Services you can create private DNS zones that are collections for holding domain names, create DNS resource records under these DNS zones, and specify access controls that are used for the DNS resolution of resource records on a zone-wide level.| [More information](/docs/dns-svcs?topic=dns-svcs-health-check-events) |
{: caption="List of IBM Cloud Networking services" caption-side="top"}



## Security services
{: #security}

The following table lists Cloud services that send logs to {{site.data.keyword.logs_routing_full_notm}}:


| Service     | Description | More information |
|-------------|-------------|-------------------------------------------------------------------------|
| {{site.data.keyword.secrets-manager_full_notm}} | With {{site.data.keyword.secrets-manager_full_notm}}, you can create, lease, and centrally manage secrets that are used in {{site.data.keyword.cloud_notm}} services or your custom-built applications. | [More information](/docs/secrets-manager?topic=secrets-manager-service-logs) |
{: caption="List of security Cloud services" caption-side="top"}


