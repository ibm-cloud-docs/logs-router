---

copyright:
  years:  2023, 2024
lastupdated: "2024-10-29"

keywords:

subcollection: logs-router

---

{{site.data.keyword.attribute-definition-list}}

# Securing your data
{: #mng-data}

So you can securely manage your data when you use {{site.data.keyword.logs_routing_full}}, you must know what data is stored, how the data is encrypted, and how you can delete any stored data.
{: shortdesc}

## What data is stored in {{site.data.keyword.logs_routing_full_notm}}
{: #mng-data-stored}

{{site.data.keyword.logs_routing_full_notm}} stores configuration data only. Stored configuration data is limited to the information that you supply when a tenant is created or updated in the service.

{{site.data.keyword.logs_routing_full_notm}} does not store any log data.


## How your data is stored and encrypted
{: #data-storage}

### Configuration data
{: #mng-data-storage-config}


{{site.data.keyword.logs_routing_full_notm}} stores the configuration data for your tenants. Tenant data that is stored in one region is not copied to any other region. Both public and private connections to the management API are encrypted by using TLS 1.2.

The storage where the configuration is stored is encrypted with LUKS by using AES-256.

Any stored credentials (such as {{site.data.keyword.la_short}} ingestion keys) are individually secured with envelope encryption by using AES-256 and an encryption key that is owned and managed by {{site.data.keyword.logs_routing_full_notm}}.

### Log data
{: #mng-data-storage-logs}

Log data that is routed by {{site.data.keyword.logs_routing_full_notm}} is secured by using a private connection. The connection supports TLS 1.2.

Log data is routed to a destination such as an {{site.data.keyword.logs_full_notm}} instance or an {{site.data.keyword.la_full_notm}} instance. You manage the instance and the data that is collected in the instance. For example, for more information about {{site.data.keyword.la_full_notm}} data security, see [Data security](/docs/log-analysis?topic=log-analysis-mng-data).
{: note}


## Deleting your data
{: #mng-data-storage-delete}

### Configuration data
{: #mng-data-storage-delete-config}

{{site.data.keyword.logs_routing_full_notm}} stores configuration data only.

To stop {{site.data.keyword.logs_routing_full_notm}} from routing logs to a destination, you must delete the tenant. For more information, see [Delete the tenant](/docs/logs-router?topic=logs-router-tenant-delete&interface=ui).


To completely delete all the configuration data of the account, complete the following steps:

1. [Delete the tenant.](/docs/logs-router?topic=logs-router-tenant-delete&interface=ui)
2. Open an IBM support ticket to request deletion of all your service metadata. For more information about opening an IBM support ticket, or about support levels and ticket severities, see [Creating support cases](/docs/account?topic=account-open-case&interface=ui).



### Log data
{: #mng-data-storage-delete-logs}

The {{site.data.keyword.logs_routing_full_notm}} service routes data to 1 or more destinations. It does not store log data. You must follow the guidance that is provided by each destination type to delete log data.
