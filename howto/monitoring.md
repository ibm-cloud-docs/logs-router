---

copyright:
  years: 2024
lastupdated: "2024-05-21"

keywords: 

subcollection: logs-router

---

{{site.data.keyword.attribute-definition-list}}

# Monitoring metrics for {{site.data.keyword.logs_routing_full_notm}} (WIP)
{: #monitoring}

{{site.data.keyword.mon_full}} is a third-party cloud-native, and container-intelligence management system that you can include as part of your {{site.data.keyword.cloud_notm}} architecture. Use it to gain operational visibility into the performance and health of your applications, services, and platforms. It offers administrators, DevOps teams and developers full stack telemetry with advanced features to monitor and troubleshoot, define alerts, and design custom dashboards. {{site.data.keyword.logs_routing_full_notm}} sends metric data to {{site.data.keyword.mon_full_notm}}.
{: shortdesc}


## Platform metrics overview
{: #platform_metrics}

You can configure only 1 instance of the {{site.data.keyword.mon_full_notm}} service per region to collect platform metrics.
* To configure the {{site.data.keyword.mon_full_notm}} instance, you must [enable the instance that will receive platform metrics.](/docs/monitoring?topic=monitoring-platform_metrics_enabling)
* If a {{site.data.keyword.mon_full_notm}} instance in a region is already enabled to collect platform metrics, metrics from {{site.data.keyword.mon_full_notm}} enabled services are collected automatically and available for monitoring through this instance. For more information about enabled services, see [Services generating metrics](/docs/monitoring?topic=monitoring-cloud_services).

To monitor platform metrics, check that the {{site.data.keyword.mon_full_notm}} instance is provisioned in the same region where the {{site.data.keyword.logs_routing_full_notm}} instance is provisioned.
{: important}

## Enabling platform metrics from {{site.data.keyword.logs_routing_full_notm}}
{: #monitoring-enable}

<!-- If your service requires specific configuration steps to enable platform metrics, include those steps in this section. -->

## Viewing metrics
{: #monitoring-view}

To monitor {{site.data.keyword.logs_routing_full_notm}} metrics, you must launch the {{site.data.keyword.mon_full_notm}} web UI for the instance that is enabled for platform metrics in the region where your {{site.data.keyword.logs_routing_full_notm}} instance is provisioned.
{: important}

### Launching monitoring web UI from the Observability page
{: #monitoring-view-ob}

For information on launching the {{site.data.keyword.mon_full_notm}} UI, see [Launching the UI in the {{site.data.keyword.mon_full_notm}} documentation.](/docs/monitoring?topic=monitoring-launch)

## Monitoring {{site.data.keyword.logs_routing_full_notm}}
{: #monitoring-monitor}

<!-- Describe any monitoring considerations specific to your service. -->

### _Add entries per recommendation to monitor your service_
{: #monitoring-monitor-1}

<!-- Add areas customers might want to monitor including a description of what should be monitored and any user actions the customer might want to take.  Repeat subsections as needed. -->

## serviceName predefined dashboards
{: #monitoring-dashboards}

<!-- If your service has predefined IBM Cloud Monitoring dashboards, describe them here. -->

## Metrics available by service plan
{: #monitoring-metrics-by-plan}

<!-- If your service's metrics are only available for specific service plans, describe the metrics and the applicable service plans here. If this section does not apply, remove it for your service. -->

## Predefined alerts
{: #monitoring-alerts}

<!-- If your service has any predefined alerts, describe them here. If this section does not apply, remove it for your service -->

## {{site.data.keyword.logs_routing_full_notm}} metrics dictionary
{: #monitoring-metrics-dictionary}

<!-- Include the generated metrics documentation provided by the tool described here: https://test.cloud.ibm.com/docs/observability-ibm?topic=observability-ibm-documentation-metric-dictionary -->

## Attributes for segmentation
{: #monitoring-attributes}

### Global attributes
{: #monitoring-attributes-global}

<!-- Describe the attributes that are available for segmenting all metrics -->

### Additional attributes
{: #monitoring-attributes-add}

<!-- Describe the attributes that are available to segment one or more attributes. -->

