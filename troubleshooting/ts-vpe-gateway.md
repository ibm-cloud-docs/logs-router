---

copyright:
  years: 2024
lastupdated: "2024-06-21"

keywords:

subcollection: logs-router

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Are you having trouble connecting to {{site.data.keyword.logs_routing_full_notm}} using the VPE?
{: #ts-vpe-gw-setup}
{: troubleshoot}
{: support}

{{site.data.keyword.logs_routing_full_notm}} supports ingestion only via private endpoints.
When using Gen2 infrastructure, the recommended way to connect is using a VPE gateway,
although CSE proxy is also supported.
When using classic infrastructure, VPE is not supported and only the CSE proxy can be used.
{: shortdesc}

When the VPE gateway is not set up correctly, this will result in network errors when trying to connect.
{: tsSymptoms}

VPE gateways are only available when using Gen2 infrastructure, and need to be created before connection will be possible.
{: tsCauses}

Check that you are using Gen2 infrastructure. If you are using classic infrastructure, the CSE proxy endpoints have to be used.
Next, ensure that the VPE gateway between {{site.data.keyword.logs_routing_full_notm}} and your VPC has been correctly created.
{: tsResolve}
