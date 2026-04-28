---

copyright:
  years:  2023, 2026
lastupdated: "2026-04-28"

keywords:

subcollection: logs-router

---

{{site.data.keyword.attribute-definition-list}}


# Configuring account settings
{: #settings-about}

You can configure {{site.data.keyword.logs_routing_full_notm}} account settings to define where and how platform logs are collected, routed, and managed in your account. You can do it through the UI, or by using the CLI, the REST API V3, or Terraform scripts.
{: shortdesc}

When you configure or modify the {{site.data.keyword.logs_routing_full_notm}} account settings, consider the following information:

Every time you modify the {{site.data.keyword.logs_routing_full_notm}} account settings, the data that is passed in the new request replaces any existing configuration data. You must ensure that any existing data is not deleted when you run an update of the account settings by including it in the new request.
{: important}

Before you disable public endpoints by setting `--private-api-endpoint-only TRUE`, make sure your account has access to the private endpoint.  You can do this by running the command `ibmcloud account show`.  If `VRF Enabled` is `true` and `Service Endpoint Enabled` is `true` then you have access to the private endpoint.  If you do not have access to the private endpoint, you will be unable to re-enable the public endpoint since private endpoint access is required to re-enable the public endpoint.
{: important}

You can use private and public endpoints to manage settings. For more information about the list of `ENDPOINTS` that are available, see [Endpoints](/docs/logs-router?topic=logs-router-locations).

- Through the private network, you must use an API endpoint with the following format: `https://api.private.<region>.logs-router.cloud.ibm.com`

- Through the public network, you must use an API endpoint with the following format: `https://api.<region>.logs-router.cloud.ibm.com`


## What data can you configure through the {{site.data.keyword.logs_routing_full_notm}} account settings?
{: #settings-about-what}

You can define any of the following information:

*  The location in your {{site.data.keyword.cloud_notm}} account where the {{site.data.keyword.logs_routing_full_notm}} account configuration metadata is stored.

    By metadata, we refer to the target/route/settings data that is available across the account in any region.

    You can choose any of the supported locations where {{site.data.keyword.logs_routing_full_notm}} is available. For more information, see [Locations](/docs/logs-router?topic=logs-router-locations).

    You can choose a location where the data is stored. You can also configure a backup location where the metadata is stored for recovery purposes.

    Take into account any corporate or industry compliance requirements such as Financial Services Validated locations, or EU-managed regions.

    You must configure the primary metadata region before creating targets and routes ensuring the configuration meets your compliance needs.

* The type of endpoints that are allowed to manage the {{site.data.keyword.logs_routing_full_notm}} account configuration in the account.

    By default, public and private endpoints are enabled.

    You can configure your account to restrict management through private endpoints only.

* The locations where an account administrator can define targets to collect logs.

    You can choose any of the supported locations where {{site.data.keyword.logs_routing_full_notm}} is available. For more information, see [Locations](/docs/logs-router?topic=logs-router-regions&interface=cli).

    Take into account any corporate or industry compliance requirements such as Financial Services Validated locations, or EU-managed regions.

* Default target locations, that is, 1 or more targets in the account, that will collect logs from supported {{site.data.keyword.logs_routing_full_notm}} locations where you have not configured how you want to collect logs.

   If you define more than 1 target, all default targets get a copy of logs that do not have a routing rule to indicate where to collect them in the account. You can define up to 2 default targets per account.
   {: note}

* The API version that is enabled in the account for {{site.data.keyword.logs_routing_full_notm}}. Valid values are: `V1` or `V3`.


## IAM permissions
{: #settings-about-access}

Ensure you have the [correct IAM permissions to configure {{site.data.keyword.logs_routing_full_notm}} settingss.](/docs/logs-router?topic=logs-router-iam)




## Managing settings using the UI
{: #settings_ui}
{: ui}

You can manage your settings definition using the {{site.data.keyword.logs_routing_full_notm}} UI. For more information, see [Configuring account settings](/docs/logs-router?topic=logs-router-settings&interface=ui).





## CLI commands
{: #settings-about-cli}
{: cli}

The following table lists the actions that you can run to manage settings:

| Action                     | Command                                         |
|----------------------------|--------------------------------------------------|
| Get settings information   | `ibmcloud logs-router setting get`        |
| Update settings            | `ibmcloud logs-router setting update`   |
{: caption="Settings actions by using the {{site.data.keyword.logs_routing_full_notm}} CLI" caption-side="top"}


For more information, see [{{site.data.keyword.logs_routing_full_notm}} CLI](/docs/logs-router?topic=logs-router-logs-router-cli).


## API methods
{: #settings-about-api}
{: api}


The following table lists the actions that you can run to manage settings:

| Action                     | REST API Method  | API_URL                                          |
|----------------------------|------------------|--------------------------------------------------|
| Get settings information   | `GET`            | `<ENDPOINT>/v3/settings`              |
| Update settings            | `PATCH`            | `<ENDPOINT>/v3/settings`  |
{: caption="Settings actions by using the {{site.data.keyword.logs_routing_full_notm}} REST API" caption-side="top"}


For more information about the REST API, see [the settings API](https://{DomainName}/apidocs/logs-router-service-api/logs-router-v3#get-settings){: external}.
{: note}



### HTTP response codes
{: #settings-about-rc}
{: api}


When you use the {{site.data.keyword.logs_routing_full_notm}} REST API, you can get standard HTTP response codes to indicate whether a method completed successfully.

- A 200 response always indicates success.
- A 4xx response indicates a failure.
- A 5xx response usually indicates an internal system error.

See the following table for some HTTP response codes:

| Status code | Status | Description |
|-------------|--------|-------------|
| `200` |	OK | The request was successful. |
| `201` |	OK | The request was successful. A resource is created. |
| `400` | Bad Request |	The request was unsuccessful. You might be missing a parameter that is required. |
| `401` | Unauthorized | The IAM token that is used in the API request is invalid or expired. |
| `403` | Forbidden | The operation is forbidden due to insufficient permissions. |
| `404` | Not Found |	The requested resource doesn't exist or is already deleted. |
| `409`|  Conflict | There is a conflict with the request data and the state of resources in system. |
| `429` | Too Many Requests |	Too many requests hit the API too quickly. |
| `500` | Internal Server Error |	Something went wrong. Your request could not be processed. Try again later. If the problem persists, note the transaction-id in the response header and contact [IBM Cloud support](https://watson.service-now.com/wcp).|
{: caption="List of HTTP response codes" caption-side="top"}
