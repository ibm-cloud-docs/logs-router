---

copyright:
  years:  2023, 2024
lastupdated: "2024-08-29"

keywords:

subcollection: logs-router

---

{{site.data.keyword.attribute-definition-list}}

# Adding a target to a tenant
{: #tenant-add-target}

You can list existing tenants in {{site.data.keyword.logs_routing_full}} that are defined in the account.
{: shortdesc}







{: #migration-add-logs-target-api} {: api}

To add a target, you must supply information about the destination where you want your logs delivered. Follow the instructions in: Retrieving your {{site.data.keyword.logs_full_notm}} information and creating a service to service authorization.

Run the following command to add a target sending logs to {{site.data.keyword.logs_full_notm}} to a tenant which already has a target sending logs to {{site.data.keyword.la_full_notm}}:

curl -X POST https://<MANAGEMENT-API-ENDPOINT>:<PORT>/v1/tenants/<TENANT-ID>/targets \
-H "Content-Type: application/json" \
-H "Authorization: ${IAM_TOKEN}" \
-H 'IBM-API-Version: CURRENT_DATE' \
--data '{
        "log_sink_crn": "LOG_SINK_CRN",
        "name": "TARGET_NAME",
        "parameters": {
            "host": "LOG_SINK_INGESTION_ENDPOINT",
            "port": LOG_SINK_PORT
        }'

{: codeblock}

Where

    <MANAGEMENT-API-ENDPOINT> is the {{site.data.keyword.logs_routing_full}} endpoint in the region where you plan to collect logs. For more information, see Endpoints. Make sure to use the corresponding port in <PORT>.
    <TENANT-ID>: The ID of your existing tenant where you want to add a target.
    LOG_SINK_CRN: CRN of the target {{site.data.keyword.logs_full_notm}} instance.
    LOG_SINK_INGESTION_ENDPOINT: Full qualified ingestion endpoint for the log-sink.
    LOG_SINK_PORT: The corresponding port to the LOG_SINK_INGESTION_ENDPOINT.
    CURRENT_DATE: Specify the current date to request the latest version of the API. The valid format is YYYY-MM-DD. Any date up to the current date can be provided.

The following shows an example of adding a target in us-east using the public endpoint and an existing tenant with ID 97543c-77b7-eg23-8114-999b31a2b3:

curl -X POST "https://management.us-east.logs-router.cloud.ibm.com/v1/tenants/97543c-77b7-eg23-8114-999b31a2b3/targets" \
--header "Authorization: Bearer TOKEN" \
--header 'Content-Type: application/json' \
--header 'IBM-API-Version: 2024-03-01' \
--data '{
            "log_sink_crn": "crn:v1:bluemix:public:logs:us-east:a/3516b8fa0a174a71899f5affa4f18d78:3517d2ed-9429-af34-ad52-34278391cbc8::",
            "name": "my-log-sink",
            "parameters": {
                "host": "743e3b4f-72bc-4669-9fa6-0e6621d0b232.ingress.eu-es.logs.cloud.ibm.com",
                "port": 443
                }
        }'

{: pre}

If the creation request was successful, a response that contains your tenant metadata is returned.
