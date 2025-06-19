---

copyright:
  years:  2023, 2025
lastupdated: "2025-06-19"

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


## Analytics services
{: #analytics}

The following table lists services that send logs to {{site.data.keyword.logs_routing_full_notm}}:

| Service     | Description | More information |
|-------------|-------------|-------------------|
| {{site.data.keyword.iae_full_notm}} | The {{site.data.keyword.iae_full_notm}} Standard Serverless plan for Apache Spark offers the ability to spin up {{site.data.keyword.iae_full_notm}} serverless instances within seconds, customize them with library packages of your choice, and run your Spark workloads. | [More information](/docs/AnalyticsEngine?topic=AnalyticsEngine-platform-logs) |
| {{site.data.keyword.openpages_full_notm}} | {{site.data.keyword.openpages_full_notm}} provides an AI-powered, highly scalable, governance, risk and compliance (GRC) solution that can run on any cloud. | [More information](/docs/openpages?topic=openpages-logging) |
{: caption="List of analytics services" caption-side="top"}



## Compute serverless services
{: #serverless}

The following table lists services that send logs to {{site.data.keyword.logs_routing_full_notm}}:

| Service     | Description | More information |
|-------------|-------------|-------------------|
| {{site.data.keyword.satellitelong_notm}} | With {{site.data.keyword.satellitelong_notm}}, you can bring your own compute infrastructure to run {{site.data.keyword.cloud_notm}} services and consistently deploy, manage, and control your app workloads. | [More information](/docs/satellite?topic=satellite-health) |
| {{site.data.keyword.codeenginefull_notm}}  | {{site.data.keyword.codeenginefull_notm}} is a fully managed, serverless platform that runs your containerized workloads, including web apps, micro-services, event-driven functions, or batch jobs. | [More information](/docs/codeengine?topic=codeengine-logging&interface=ui) |
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
| {{site.data.keyword.Db2_on_Cloud_long}} | {{site.data.keyword.Db2_on_Cloud_long}} is a managed Db2 service that is hosted in the {{site.data.keyword.cloud_notm}} and integrated with other {{site.data.keyword.cloud_notm}} services. | [More information](/docs/Db2onCloud?topic=Db2onCloud-log_mon) |
| {{site.data.keyword.databases-for-postgresql_full}} | {{site.data.keyword.databases-for-postgresql_full_notm}} is a powerful, open source object-relational database that is highly customizable. | [More information](/docs/databases-for-postgresql?topic=databases-for-postgresql-logging) |
| {{site.data.keyword.databases-for-redis_full}} | {{site.data.keyword.databases-for-redis_full_notm}} is a blazingly fast, in-memory data structure store. | [More information](/docs/databases-for-redis?topic=databases-for-redis-logging) |
| {{site.data.keyword.databases-for-etcd_full}} | {{site.data.keyword.databases-for-etcd_full_notm}} is a distributed, reliable key-value store for the most critical data of a distributed system. | [More information](/docs/databases-for-etcd?topic=databases-for-etcd-logging) |
| {{site.data.keyword.databases-for-elasticsearch_full}} | {{site.data.keyword.databases-for-elasticsearch_full_notm}} combines the power of a full text search engine with the indexing strengths of a JSON document database. | [More information](/docs/databases-for-elasticsearch?topic=databases-for-elasticsearch-logging) |
| {{site.data.keyword.messages-for-rabbitmq_full}} | {{site.data.keyword.messages-for-rabbitmq_full_notm}} is an open source multi-protocol messaging broker. | [More information](/docs/messages-for-rabbitmq?topic=messages-for-rabbitmq-logging) |
| {{site.data.keyword.databases-for-mongodb_full_notm}} | {{site.data.keyword.databases-for-mongodb_full_notm}} is a JSON document store with a rich query and aggregation framework. | [More information](/docs/databases-for-mongodb?topic=databases-for-mongodb-logging) |
| {{site.data.keyword.databases-for-mysql_full}} | {{site.data.keyword.databases-for-mysql_fullnotm}} is a serverless, cloud database service that is fully integrated into the IBM Cloud environment. Use IBM Cloud® Databases for MySQL to use a cloud database system without purchasing and setting up your own hardware, installing your own database software, or managing the database yourself. | [More information](/docs/databases-for-mysql?topic=databases-for-mysql-logging) |
| {{site.data.keyword.databases-for-enterprisedb_full}} | {{site.data.keyword.databases-for-enterprisedb_full_notm}} is a PostgreSQL-based database engine optimized for performance, developer productivity, and compatibility with Oracle. | [More information](/docs/databases-for-enterprisedb?topic=databases-for-enterprisedb-logging) |
{: caption="List of database services" caption-side="top"}


## Developer tools
{: #developer_tools}

The following table lists services that send logs to {{site.data.keyword.logs_routing_full_notm}}:

| Service     | Description | More information |
|-------------|-------------|-----------|
| {{site.data.keyword.en_full}} | {{site.data.keyword.en_short}} is a routing service that tells you about critical events that occur in your {{site.data.keyword.cloud}} account. You can filter and route event notifications from {{site.data.keyword.cloud_notm}} services like Monitoring, Security and Compliance Center, and Secrets Manager to communication channels like email, SMS, push notifications, webhook, slack, and Microsoft&trade; Teams.  | [More information](/docs/event-notifications?topic=event-notifications-logging)  |
{: caption="List of Developer tools services" caption-side="top"}



## Integration services
{: #integration}

The following table lists services that send logs to {{site.data.keyword.logs_routing_full_notm}}:

| Service     | Description | More information |
|-------------|-------------|-------------------------------------------------------------------------|
| {{site.data.keyword.mq_short}} | MQ on IBM Cloud enables you to quickly and easily deploy queue managers in the cloud and connect your applications to them, for reliable data transfer between different parts of your enterprise application landscape. | [More information](/docs/mqcloud?topic=mqcloud-logging) |
| {{site.data.keyword.apiconnect_full}} | {{site.data.keyword.apiconnect_full}} makes it easy to create, securely expose, manage, and monetize APIs so that you and your customers can power digital applications and spur innovation. | [More information](/docs/apiconnect?topic=apiconnect-logging) |
{: caption="List of integration Cloud services" caption-side="top"}



## VPC services
{: #vpc}

The following table lists Cloud services that send logs to {{site.data.keyword.logs_routing_full_notm}}:


| Service          | Description | More information         |
|------------------|-------------|-------------------|
| Load Balancer for VPC | Use IBM Cloud Load Balancer for VPC to distribute traffic among multiple server instances within the same region of your VPC. | [More information](/docs/vpc?topic=vpc-datapath-logging) |
| VPN | Use this service to connect private networks in a secure fashion. You can use VPN to set up an IPsec site-to-site tunnel between your VPC and your on-premise private network or another VPC. | [More information](/docs/vpc?topic=vpc-logging#logging-s2s) |
| Client VPN for VPC | Set up and configure your clients' VPN environment to connect to the VPN server. | [More information](/docs/vpc?topic=vpc-logging#logging-c2s) |
| Dedicated Host for VPC | You can create a dedicated host to carve out a single-tenant compute node, free from users outside of your organization.  | [More information](/docs/vpc?topic=vpc-logging#logging-dedicated-host) |
| Flow Log Collector| This service is used to collect and store information regarding the Internet Protocol (IP) traffic going to and from network interfaces within your Virtual Private Cloud (VPC) |[More information](/docs/vpc?topic=vpc-logging#logging-flow-log-collector_msgs) |
| Snapshots for VPC| This offering is used to create a point-in-time copy of your boot or data volume. |[More information](/docs/vpc?topic=vpc-logging&interface=ui#logging-snapshots) |
| {{site.data.keyword.filestorage_vpc_short}}| {{site.data.keyword.filestorage_vpc_short}} provides NFS-based file storage services within the VPC Infrastructure. |[More information](/docs/vpc?topic=vpc-logging&interface=ui#logging-file-share-replication) |
{: caption="List of IBM Cloud VPC services" caption-side="top"}



## Classic Infrastructure services
{: #classic_services}

The following table lists Classic Infrastructure services that send logs to {{site.data.keyword.logs_routing_full_notm}}:

| Service          | Description | More information         |
|------------------|-------------|-------------------|
| IBM Cloud Load Balancer | Use this service to improve availability of business-critical applications by distributing traffic among multiple application server instances, and by forwarding traffic to healthy instances only. | [More information](/docs/loadbalancer-service?topic=loadbalancer-service-ibm-cloud-logging) |
{: caption="List of Classic Infrastructure services" caption-side="top"}

## Networking services
{: #networking}

The following table lists Cloud Networking services that send logs to {{site.data.keyword.logs_routing_full_notm}}:


| Service          | Description | More information         |
|------------------|-------------|-------------------|
| {{site.data.keyword.dns_full_notm}} | With DNS Services you can create private DNS zones that are collections for holding domain names, create DNS resource records under these DNS zones, and specify access controls that are used for the DNS resolution of resource records on a zone-wide level.| [More information](/docs/dns-svcs?topic=dns-svcs-logging) |
{: caption="List of IBM Cloud Networking services" caption-side="top"}


## Security services
{: #security}

The following table lists Cloud services that send logs to {{site.data.keyword.logs_routing_full_notm}}:


| Service     | Description | More information |
|-------------|-------------|-------------------------------------------------------------------------|
| {{site.data.keyword.secrets-manager_full_notm}} | With {{site.data.keyword.secrets-manager_full_notm}}, you can create, lease, and centrally manage secrets that are used in {{site.data.keyword.cloud_notm}} services or your custom-built applications. | [More information](/docs/secrets-manager?topic=secrets-manager-logging) |
| {{site.data.keyword.compliance_short}} | With {{site.data.keyword.compliance_short}}, you can continuously evaluate your resource configurations for compliance. | [More information](/docs/security-compliance?topic=security-compliance-logging) |
{: caption="List of security Cloud services" caption-side="top"}

## VMware Solutions services
{: #vmware-solutions}

The following table lists VMware Solutions services that send logs to {{site.data.keyword.logs_routing_full_notm}}:

| Service     | Description | More information |
|-------------|-------------|------------------|
| {{site.data.keyword.vmwaresolutions_full}} | {{site.data.keyword.vmwaresolutions_short}} is a group of offerings that provide deployment and management of VMware® by Broadcom virtualized environments. | [More information](/docs/vmwaresolutions?topic=vmwaresolutions-logging) |
{: caption="List of VMware Solutions services" caption-side="top"}


## Watson services
{: #watson}

The following table lists Cloud services that send logs to {{site.data.keyword.logs_routing_full_notm}}:


| Service     | Description | More information |
|-------------|-------------|-------------------------------------------------------------------------|
| {{site.data.keyword.conversationshort}} | {{site.data.keyword.conversationshort}}, which focuses on actions to build customer conversations, is simple enough for anyone to build a virtual assistant.| [More information](/docs/watson-assistant?topic=watson-assistant-getting-started) |
| {{site.data.keyword.discoveryfull}} | {{site.data.keyword.discoveryfull}} is an intelligent document processing engine that helps you to gain insights from complex business documents.| [More information](/docs/discovery-data?topic=discovery-data-getting-started) |
| {{site.data.keyword.knowledgestudiofull}} | Use {{site.data.keyword.knowledgestudiofull}} to create a machine learning model that understands the linguistic nuances, meaning, and relationships specific to your industry, or to create a rule-based model that finds entities in documents based on rules that you define. | [More information](/docs/watson-knowledge-studio?topic=watson-knowledge-studio-wks_overview_full) |
| {{site.data.keyword.nlufull}} | With {{site.data.keyword.nlufull}}, developers can analyze semantic features of text input, including categories, concepts, emotion, entities, keywords, metadata, relations, semantic roles, and sentiment. | [More information](/docs/natural-language-understanding?topic=natural-language-understanding-about) |
| {{site.data.keyword.speechtotextfull}} | The {{site.data.keyword.speechtotextfull}} service transcribes audio to text to enable speech transcription capabilities for applications. | [More information](/docs/speech-to-text?topic=speech-to-text-gettingStarted) |
| {{site.data.keyword.texttospeechfull}} | The {{site.data.keyword.texttospeechfull}} service converts written text to natural-sounding speech to provide speech-synthesis capabilities for applications. | [More information](/docs/text-to-speech?topic=text-to-speech-about) |
{: caption="List of Watson services" caption-side="top"}
