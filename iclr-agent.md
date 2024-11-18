---

copyright:
  years:  2023, 2024
lastupdated: "2024-11-18"

keywords:

subcollection: logs-router

---

{{site.data.keyword.attribute-definition-list}}

# Using the {{site.data.keyword.logs_routing_full}} agent to send logs
{: #iclr-agent}

The {{site.data.keyword.logs_routing_full}} agent can be used to send application logs to {{site.data.keyword.logs_full_notm}} using the {{site.data.keyword.logs_routing_full_notm}} service. However, this is no longer the preferred method. See the information about sending data to {{site.data.keyword.logs_full_notm}} in the [{{site.data.keyword.logs_full_notm}} documentation](/docs/cloud-logs) for details about the preferred methods of sending log data to {{site.data.keyword.logs_full_notm}}.
{: shortdesc}

The information in this topic is provided as a reference for those still using the old {{site.data.keyword.logs_routing_full_notm}} agent to send logs to {{site.data.keyword.logs_full_notm}}. This method should no longer be used for new implementations.
{: important}


## About the {{site.data.keyword.logs_routing_full_notm}} agent 
{: #iclr-agent-about}

You can send logs to a target destination by using the ingestion API. The ingestion API supports only private endpoints and is therefore not accessible from the public internet.


## {{site.data.keyword.logs_routing_full_notm}} agent private ingestion endpoints
{: #iclr-agent-endpoint}

| Geography | Region                           | Ingestion Endpint |
|-----------|----------------------------------|-------------------|
| Asia Pacific  | Osaka (`jp-osa`) | `ingester.private.jp-osa.logs-router.cloud.ibm.com` |
| Asia Pacific  | Sydney (`au-syd`) | `ingester.private.au-syd.logs-router.cloud.ibm.com` |
| Asia Pacific  | Tokyo (`jp-tok`) | `ingester.private.jp-tok.logs-router.cloud.ibm.com` |
| Europe  | Frankfurt (`eu-de`) | `ingester.private.eu-de.logs-router.cloud.ibm.com` |
| London  | London (`eu-gb`) | `ingester.private.eu-gb.logs-router.cloud.ibm.com` |
| Europe  | Madrid (`eu-es`) | `ingester.private.eu-es.logs-router.cloud.ibm.com` |
| North America  | Dallas (`us-south`) | `ingester.private.us-south.logs-router.cloud.ibm.com` |
| North America  | Toronto (`ca-tor`) | `ingester.private.ca-tor.logs-router.cloud.ibm.com` |
| North America  | Washington, D.C (`us-east`) | `ingester.private.us-east.logs-router.cloud.ibm.com` |
| South America  | Sao Paulo (`br-sao`) | `ingester.private.br-sao.logs-router.cloud.ibm.com` |
{: caption="List of {{site.data.keyword.logs_routing_full_notm}} private ingestion endpoints" caption-side="top"}


## {{site.data.keyword.logs_routing_full_notm}} agent ingestion CRNs
{: #iclr-agent-crn}

The following table lists the CRNs that you can select when you create a VPE gateway by using the API to enable a virtual private ingestion endpoint:

| Region | Cloud Resource Name (CRN) |
|-----------------|-----------------|
| Dallas (`us-south`) | `crn:v1:bluemix:public:logs-router:us-south:::endpoint:ingester.private.us-south.logs-router.cloud.ibm.com` |
| Frankfurt (`eu-de`) | `crn:v1:bluemix:public:logs-router:eu-de:::endpoint:ingester.private.eu-de.logs-router.cloud.ibm.com` |
| London (`eu-gb`) | `crn:v1:bluemix:public:logs-router:eu-gb:::endpoint:ingester.private.eu-gb.logs-router.cloud.ibm.com` |
| Madrid (`eu-es`) | `crn:v1:bluemix:public:logs-router:eu-es:::endpoint:ingester.private.eu-es.logs-router.cloud.ibm.com` |
| Osaka (`jp-osa`) | `crn:v1:bluemix:public:logs-router:jp-osa:::endpoint:ingester.private.jp-osa.logs-router.cloud.ibm.com` |
| Sao Paulo (`br-sao`) | `crn:v1:bluemix:public:logs-router:br-sao:::endpoint:ingester.private.br-sao.logs-router.cloud.ibm.com` |
| Sydney (`au-syd`) | `crn:v1:bluemix:public:logs-router:au-syd:::endpoint:ingester.private.au-syd.logs-router.cloud.ibm.com` |
| Tokyo (`jp-tok`) | `crn:v1:bluemix:public:logs-router:jp-tok:::endpoint:ingester.private.jp-tok.logs-router.cloud.ibm.com` |
| Toronto (`ca-tor`) | `crn:v1:bluemix:public:logs-router:ca-tor:::endpoint:ingester.private.ca-tor.logs-router.cloud.ibm.com` |
| Washington DC (`us-east`) | `crn:v1:bluemix:public:logs-router:us-east:::endpoint:ingester.private.us-east.logs-router.cloud.ibm.com` |
{: caption="Region availability and Cloud Resource Names for connecting {{site.data.keyword.logs_routing_full_notm}} over {{site.data.keyword.cloud_notm}} private networks" caption-side="bottom"}


## Activity traking events related to the {{site.data.keyword.logs_routing_full_notm}} agent
{: #iclr-agent-at}

### Data events
{: #at_actions_data}

| Action                      | Description                                                                                  |
|-----------------------------|----------------------------------------------------------------------------------------------|
| `logs-router.event.send`    | This event is generated whenever the ingester receives a new connection request from an agent. |
{: caption="Actions that generate data events" caption-side="bottom"}


## Understanding your responsibilities
{: #iclr-agent-yr}

Agent management includes all tasks related to the installation, configuration, and upgrade of the {{site.data.keyword.logs_routing_full_notm}} agent.

| Task  | {{site.data.keyword.IBM_notm}} Responsibilities | Your Responsibilities |
|----------|-----------------------|--------|
| Installation | Provide installation packages and current documentation for supported platforms. | Deploy agent following documented instructions. |
| Configuration | Provide base configuration files for supported platforms. | Configure agent following documented instructions. |
| Configuration | Provide list of included plug-ins and links to documentation. | Read and understand documentation for any plug-ins used with agent. |
| Upgrade | Provide installation packages and documentation which can be used to upgrade the deployed agents. | Upgrade the agent following documented instructions. |
| Maintain {{site.data.keyword.cloud_notm}} high availability SLA for {{site.data.keyword.logs_routing_full_notm}}   | Provide {{site.data.keyword.logs_routing_full_notm}} functionality across availability zones in a Multi-Zone Region (MZR).    \n  \n Provide replication, fail-over features, and infrastructure maintenance and updates. | Keep your {{site.data.keyword.logs_routing_full_notm}} agent configuration in a version control system so that you can reconfigure a log source if needed.   \n  \n Monitor agent availability and ensure availability of resources for agent. |
| Updates to {{site.data.keyword.logs_routing_full_notm}} | Provide major, minor, and patch version updates for {{site.data.keyword.logs_routing_full_notm}} interfaces.   \n  \n Provide major, minor, and patch version updates for the {{site.data.keyword.logs_routing_full_notm}} agent.  \n  \n Provide notification of updates to the [agent in product release notes.](/docs/cloud-logs?topic=cloud-logs-release-notes-agent)  \n  \n Document changes in the [IBM Cloud Support Center](https://cloud.ibm.com/unifiedsupport/supportcenter) | Download and install agent updates as they become available. |
| Validate connections to target destinations by {{site.data.keyword.logs_routing_full_notm}} agents | Validate the {{site.data.keyword.logs_routing_full_notm}} agent configuration data that is used to send logs to a target destination. | Configure the {{site.data.keyword.logs_routing_full_notm}} agent with valid data and credentials for authorization and authentication. |
{: row-headers}
{: caption="Responsibilities for agent management" caption-side="bottom"}
{: summary="The first column describes the task that the customer or IBM might be responsibility for. The second column describes {{site.data.keyword.IBM_notm}} responsibilities for that task. The third column describes your responsibilities as the customer for that task."}
