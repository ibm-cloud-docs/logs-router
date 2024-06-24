---

copyright:
  years: 2022, 2024
lastupdated: "2024-06-10"

keywords: 

subcollection: logs-router

content-type: release-note

---

{{site.data.keyword.attribute-definition-list}}

# Release notes for {{site.data.keyword.logs_routing_full}}
{: #release-notes}

Use these release notes to learn about updates to {{site.data.keyword.logs_routing_full}}.
{: shortdesc}

## 10 June 2024
{: #logs-router-jun1024}
{: release-note}

Release of the {{site.data.keyword.logs_routing_full_notm}} Agent version 1.2.2
: {{site.data.keyword.logs_routing_full_notm}} Agent version 1.2.2 is available for {{site.data.keyword.containerlong_notm}}, {{site.data.keyword.openshiftlong_notm}}, RHEL8, RHEL 9, Ubuntu 20, Ubuntu 22, and Debian 11, and Debian 12.  This release is based on fluentbit [v3.0.4](https://fluentbit.io/announcements/v3.0.4/){: external}.

   This version includes the following notable changes:

   * Enables per-message acknowledgements.  With this feature, each event bundle sent by the agent to the ingester is assigned an ID, and the {{site.data.keyword.logs_routing_full_notm}} ingester will send the agent an acknowledgement once the bundle has been sent through the data plane.  This helps avoid losing log events, for example due to abnormal connection closure or network outage, since the agent will resend unacknowledged bundles.

   The following new {{site.data.keyword.logs_routing_full_notm}} Agent packages are available:

   RHEL 8

   * [https://logs-router-agent-install-packages.s3.us.cloud-object-storage.appdomain.cloud/logs-router-agent-rehl8-1.2.2.rpm](https://logs-router-agent-install-packages.s3.us.cloud-object-storage.appdomain.cloud/logs-router-agent-rhel8-1.2.2.rpm){: external}
   * [https://logs-router-agent-install-packages.s3.us.cloud-object-storage.appdomain.cloud/logs-router-agent-rhel8-1.2.2.rpm.sha256](https://logs-router-agent-install-packages.s3.us.cloud-object-storage.appdomain.cloud/logs-router-agent-rhel8-1.2.2.rpm.sha256){: external}

   RHEL 9

   * [https://logs-router-agent-install-packages.s3.us.cloud-object-storage.appdomain.cloud/logs-router-agent-1.2.2.rpm](https://logs-router-agent-install-packages.s3.us.cloud-object-storage.appdomain.cloud/logs-router-agent-1.2.2.rpm){: external}
   * [https://logs-router-agent-install-packages.s3.us.cloud-object-storage.appdomain.cloud/logs-router-agent-1.2.2.rpm.sha256](https://logs-router-agent-install-packages.s3.us.cloud-object-storage.appdomain.cloud/logs-router-agent-1.2.2.rpm.sha256){: external}

   Debian 11

   * [https://logs-router-agent-install-packages.s3.us.cloud-object-storage.appdomain.cloud/logs-router-agent-deb11-1.2.2.deb](https://logs-router-agent-install-packages.s3.us.cloud-object-storage.appdomain.cloud/logs-router-agent-deb11-1.2.2.deb){: external}
   * [https://logs-router-agent-install-packages.s3.us.cloud-object-storage.appdomain.cloud/logs-router-agent-deb11-1.2.2.deb.sha256](https://logs-router-agent-install-packages.s3.us.cloud-object-storage.appdomain.cloud/logs-router-agent-deb11-1.2.2.deb.sha256){: external}


   Ubuntu 20

   * [https://logs-router-agent-install-packages.s3.us.cloud-object-storage.appdomain.cloud/logs-routing-agent-ubuntu20-1.2.2.deb.sha256](https://logs-router-agent-install-packages.s3.us.cloud-object-storage.appdomain.cloud/logs-router-agent-ubuntu20-1.2.2.deb.sha256){: external}
   * [https://logs-router-agent-install-packages.s3.us.cloud-object-storage.appdomain.cloud/logs-router-agent-ubuntu20-1.2.2.deb](https://logs-router-agent-install-packages.s3.us.cloud-object-storage.appdomain.cloud/logs-router-agent-ubuntu20-1.2.2.deb){: external}


   Debian 12 & Ubuntu 22

   * [https://logs-router-agent-install-packages.s3.us.cloud-object-storage.appdomain.cloud/logs-router-agent-1.2.2.deb](https://logs-router-agent-install-packages.s3.us.cloud-object-storage.appdomain.cloud/logs-router-agent-1.2.2.deb){: external}
   * [https://logs-router-agent-install-packages.s3.us.cloud-object-storage.appdomain.cloud/logs-router-agent-1.2.2.deb.sha256](https://logs-router-agent-install-packages.s3.us.cloud-object-storage.appdomain.cloud/logs-router-agent-1.2.2.deb.sha256){: external}

## 29 May 2024
{: #logs-router-may2924}
{: release-note}

Release of the {{site.data.keyword.logs_routing_full_notm}} Agent version 1.2.0
: {{site.data.keyword.logs_routing_full_notm}} Agent version 1.2.0 is available for {{site.data.keyword.containerlong_notm}}, {{site.data.keyword.openshiftlong_notm}}, RHEL 9, Ubuntu 22, and Debian 12.  This release is based on fluentbit [v3.0.4](https://fluentbit.io/announcements/v3.0.4/){: external}, which includes a critical security fix for a vulnerability in the internal tracing interface. It is strongly recommended that you upgrade to this agent version.

   This version includes the following notable changes:

   * Fixes [CVE-2024-4323](https://fluentbit.io/blog/2024/05/21/statement-on-cve-2024-4323-and-its-fix/){: external}.
   * Fixes a bug in the `kubernetes` filter where the wrong labels and annotations can be applied when multiple pods or namespaces share a common base substring.
   * Enables IAM Trusted Profiles for non-containerized agents (agents running directly on Linux hosts).
   * Renames the Kubernetes `init` container to `logs-router-agent-init`.  The previous `logger-agent-init` tag can still be used, but the `logger-agent-init` tag will be disabled in the future and users are encouraged to update their deployment `yaml` file to reference the new image tag.

   The following new {{site.data.keyword.logs_routing_full_notm}} Agent packages are available:

   RHEL 9

   * [https://logs-router-agent-install-packages.s3.us.cloud-object-storage.appdomain.cloud/logs-routing-agent-1.2.0.rpm](https://logs-router-agent-install-packages.s3.us.cloud-object-storage.appdomain.cloud/logs-routing-agent-1.2.0.rpm){: external}
   * [https://logs-router-agent-install-packages.s3.us.cloud-object-storage.appdomain.cloud/logs-routing-agent-1.2.0.rpm.sha256](https://logs-router-agent-install-packages.s3.us.cloud-object-storage.appdomain.cloud/logs-routing-agent-1.2.0.rpm.sha256){: external}


   Debian 12 & Ubuntu 22

   * [https://logs-router-agent-install-packages.s3.us.cloud-object-storage.appdomain.cloud/logs-routing-agent-1.2.0.deb](https://logs-router-agent-install-packages.s3.us.cloud-object-storage.appdomain.cloud/logs-routing-agent-1.2.0.deb){: external}
   * [https://logs-router-agent-install-packages.s3.us.cloud-object-storage.appdomain.cloud/logs-routing-agent-1.2.0.deb.sha256](https://logs-router-agent-install-packages.s3.us.cloud-object-storage.appdomain.cloud/logs-routing-agent-1.2.0.deb.sha256){: external}

## 1 May 2024
{: #logs-router-may0124}
{: release-note}

Release of the {{site.data.keyword.logs_routing_full_notm}} Agent version 1.1.1
: {{site.data.keyword.logs_routing_full_notm}} Agent version 1.1.1 is available for {{site.data.keyword.containerlong_notm}}, {{site.data.keyword.openshiftlong_notm}}, RHEL 9, Ubuntu 22, and Debian 12.  This release is based on fluentbit [v3.0.2](https://fluentbit.io/announcements/v3.0.2/){: external}, which includes a critical fix for passing backpressure signals to input plugins from specific filter plugins.

   The following new {{site.data.keyword.logs_routing_full_notm}} Agent packages are available: 
   
   RHEL 9
   
   * [https://logs-router-agent-install-packages.s3.us.cloud-object-storage.appdomain.cloud/logs-routing-agent-1.1.1.rpm](https://logs-router-agent-install-packages.s3.us.cloud-object-storage.appdomain.cloud/logs-routing-agent-1.1.1.rpm){: external}

   * [https://logs-router-agent-install-packages.s3.us.cloud-object-storage.appdomain.cloud/logs-routing-agent-1.1.1.rpm.sha256](https://logs-router-agent-install-packages.s3.us.cloud-object-storage.appdomain.cloud/logs-routing-agent-1.1.1.rpm.sha256){: external}

   Debian 12 & Ubuntu 22

   * [https://logs-router-agent-install-packages.s3.us.cloud-object-storage.appdomain.cloud/logs-routing-agent-1.1.1.deb](https://logs-router-agent-install-packages.s3.us.cloud-object-storage.appdomain.cloud/logs-routing-agent-1.1.1.deb){: external}
   * [https://logs-router-agent-install-packages.s3.us.cloud-object-storage.appdomain.cloud/logs-routing-agent-1.1.1.deb.sha256](https://logs-router-agent-install-packages.s3.us.cloud-object-storage.appdomain.cloud/logs-routing-agent-1.1.1.deb.sha256){: external}


## 29 April 2024
{: #logs-router-apr2924}
{: release-note}

Availability in the Sydney, Sao Paulo, and Osaka regions
:   {{site.data.keyword.logs_routing_full_notm}} is available in the Sydney (au-syd), Sao Paulo (br-sao), and Osaka (jp-osa) regions.

   The following new {{site.data.keyword.logs_routing_full_notm}} Agent packages are available: 

   RHEL 9

   * [https://logs-router-agent-install-packages.s3.us.cloud-object-storage.appdomain.cloud/logs-routing-agent-1.1.0.rpm](https://logs-router-agent-install-packages.s3.us.cloud-object-storage.appdomain.cloud/logs-routing-agent-1.1.0.rpm){: external}
   * [https://logs-router-agent-install-packages.s3.us.cloud-object-storage.appdomain.cloud/logs-routing-agent-1.1.0.rpm.sha256](https://logs-router-agent-install-packages.s3.us.cloud-object-storage.appdomain.cloud/logs-routing-agent-1.1.0.rpm.sha256){: external}


   Debian 12 & Ubuntu 22

   * [https://logs-router-agent-install-packages.s3.us.cloud-object-storage.appdomain.cloud/logs-routing-agent-1.1.0.deb](https://logs-router-agent-install-packages.s3.us.cloud-object-storage.appdomain.cloud/logs-routing-agent-1.1.0.deb){: external}
   * [https://logs-router-agent-install-packages.s3.us.cloud-object-storage.appdomain.cloud/logs-routing-agent-1.1.0.deb.sha256](https://logs-router-agent-install-packages.s3.us.cloud-object-storage.appdomain.cloud/logs-routing-agent-1.1.0.deb.sha256){: external}


   RHEL 8

   * [https://logs-router-agent-install-packages.s3.us.cloud-object-storage.appdomain.cloud/logs-routing-agent-1.1.0-rhel8.rpm](https://logs-router-agent-install-packages.s3.us.cloud-object-storage.appdomain.cloud/logs-routing-agent-1.1.0-rhel8.rpm){: external}
   * [https://logs-router-agent-install-packages.s3.us.cloud-object-storage.appdomain.cloud/logs-routing-agent-1.1.0-rhel8.rpm.sha256](https://logs-router-agent-install-packages.s3.us.cloud-object-storage.appdomain.cloud/logs-routing-agent-1.1.0-rhel8.rpm.sha256){: external}


   Debian 11

   * [https://logs-router-agent-install-packages.s3.us.cloud-object-storage.appdomain.cloud/logs-routing-agent-1.1.0-debian11.deb](https://logs-router-agent-install-packages.s3.us.cloud-object-storage.appdomain.cloud/logs-routing-agent-1.1.0-debian11.deb){: external}
   * [https://logs-router-agent-install-packages.s3.us.cloud-object-storage.appdomain.cloud/logs-routing-agent-1.1.0-debian11.deb.sha256](https://logs-router-agent-install-packages.s3.us.cloud-object-storage.appdomain.cloud/logs-routing-agent-1.1.0-debian11.deb.sha256){: external}


   Ubuntu 20

   * [https://logs-router-agent-install-packages.s3.us.cloud-object-storage.appdomain.cloud/logs-routing-agent-1.1.0-ubuntu20.deb](https://logs-router-agent-install-packages.s3.us.cloud-object-storage.appdomain.cloud/logs-routing-agent-1.1.0-ubuntu20.deb){: external}
   * [https://logs-router-agent-install-packages.s3.us.cloud-object-storage.appdomain.cloud/logs-routing-agent-1.1.0-ubuntu20.deb.sha256](https://logs-router-agent-install-packages.s3.us.cloud-object-storage.appdomain.cloud/logs-routing-agent-1.1.0-ubuntu20.deb.sha256){: external}


## 12 April 2024
{: #logs-router-apr1224}
{: release-note}

Availability in the Toronto region
:   {{site.data.keyword.logs_routing_full_notm}} is available in the Toronto (ca-tor) region.

## 9 April 2024
{: #logs-router-apr0924}
{: release-note}

Availability in the London and Tokyo regions
:   {{site.data.keyword.logs_routing_full_notm}} is available in the London (eu-gb) and Tokyo (jp-tok) regions.

## 1 March 2024
{: #logs-router-mar0124}
{: release-note}

Availability in the Frankfurt region
:   {{site.data.keyword.logs_routing_full_notm}} is available in the Frankfurt (eu-de) region.

## 7 February 2024
{: #logs-router-feb0724}
{: release-note}

Availability in the Madrid region
:   {{site.data.keyword.logs_routing_full_notm}} is available in the Madrid (eu-es) region.

## 15 December 2023
{: #logs-router-dec1523}
{: release-note}

General availability
:   {{site.data.keyword.logs_routing_full_notm}} is generally available in the Washington (us-east) region.
