---

copyright:
   years: 2023
lastupdated: "2023-04-17"

keywords:

subcollection: secure-infrastructure-vpc

---

{{site.data.keyword.attribute-definition-list}}

# Understanding your responsibilities when you use the landing zone deployable architectures
{: #vpc-resp}

In {{site.data.keyword.cloud_notm}}, the responsibilities for deploying, operating, and securing products are shared between {{site.data.keyword.IBM_notm}} and you, the application provider. Learn about the management responsibilities and terms and conditions that you have when you use one of the landing zone deployable architectures.
{: shortdesc}

It is important for you to understand these responsibilities for every {{site.data.keyword.cloud_notm}} service that you deploy as part of the deployable architecture. For a high-level view of the service types in {{site.data.keyword.cloud}} and the breakdown of responsibilities between the customer and {{site.data.keyword.IBM_notm}} for each type, see [Shared responsibilities for {{site.data.keyword.cloud_notm}} offerings](/docs/overview?topic=overview-shared-responsibilities). This document augments and completes, but does not replace, the documentation about shared responsibilities for each service. For more information, see [Responsibilities for operating services in your deployment](/docs/framework-financial-services?topic=framework-financial-services-shared-responsibilities) in {{site.data.keyword.framework-fs_notm}}.

Review the following sections for the specific responsibilities for you and for {{site.data.keyword.IBM_notm}} when you use a deployable architecture. For the overall terms of use, see [{{site.data.keyword.cloud}} Terms and Notices](/docs/overview/terms-of-use?topic=overview-terms).

<!-- If you plan to list resource (see resources listed in each table in the platform shared responsibilities topic linked above) responsibility instead of individual tasks, you do not need to include rows for Hypervisor, Physical Servers and memory, Physical storage, Physical network and devices, and Facilities and data centers unless you need to indicate a 'Shared' or 'Customer' responsibility for one of the areas within those Resources. -->

## Incident and operations management
{: #vpc-incident-and-ops}

<!-- Use this section description exactly as worded. -->
<!-- If there is a task that is the customer's responsibility and you have associated docs for how a customer completes that task, link to it from the Your responsibilities column. -->

Incident and operations management includes tasks such as monitoring, event management, high availability, problem determination, recovery, and full state backup and recovery.

The deployable architecture does not identify specific responsibilities in this area. Incident and operations management tasks are outlined for services at [Responsibilities for operating services in your deployment](/docs/framework-financial-services?topic=framework-financial-services-shared-responsibilities) in {{site.data.keyword.framework-fs_notm}}.

## Change management
{: #vpc-change-management}

<!-- Use this section description exactly as worded. -->
<!-- If there is a task that is the customer's responsibility and you have associated docs for how a customer completes that task, link to it from the Your responsibilities column. -->

Change management includes tasks such as deployment, configuration, upgrades, patching, configuration changes, and deletion.

| Task | {{site.data.keyword.IBM_notm}} responsibilities | Your responsibilities |
|------|-------------------------------------------------|-----------------------|
| Update the deployable architecture  | Provide regular semantic-versioned updates to the deployable architecture. | Apply the provided updates. |
| Keep deployed services and resources up to date | | Apply fixes and updates to the compute resources that are created from the deployable architecture. These resources are not updated through the deployable architecture unless otherwise indicated.   \n * [Red Hat OpenShift clusters](/docs/openshift?topic=openshift-update)  \n * [Kubernetes lusters, worker nodes, and cluster components](/docs/containers?topic=containers-update) |
| Update to the next major deployable architecture release | Provide a migration path when possible. |  |
| Provide notice of end of support. | Provide notice through regular channels. | |
{: row-headers}
{: caption="Table 2. Responsibilites for change management" caption-side="bottom"}
{: summary="The rows are read from left to right. The first column describes the task that the customer or IBM might be responsible for. The second column describes {{site.data.keyword.IBM_notm}} responsibilities for that task. The third column describes your responsibilities as the customer for that task."}


## Identity and access management
{: #vpc-iam-responsibilities}

<!-- Use this section description exactly as worded. -->
<!-- If there is a task that is the customer's responsibility and you have associated docs for how a customer completes that task, link to it from the Your responsibilities column. -->

Identity and access management includes tasks such as authentication, authorization, access control policies, and approving, granting, and revoking access.

| Task | {{site.data.keyword.IBM_notm}} responsibilities | Your responsibilities |
|------|-------------------------------------------------|-----------------------|
| Secure with least priviledge | Document the minimal IAM access requirements to run the deployable architecture. |  |
| Manage secrets | | * Generate the necessary secrets (for example, IAM API keys, SSH keys) that are required for the deployable architecture.  \n * Manage generated secrets by following secure best practices. |
{: row-headers}
{: caption="Table 3. Responsibilites for identity and access management" caption-side="bottom"}
{: summary="The rows are read from left to right. The first column describes the task that the customer or IBM might be responsible for. The second column describes {{site.data.keyword.IBM_notm}} responsibilities for that task. The third column describes your responsibilities as the customer for that task."}

## Security and regulation compliance
{: #vpc-security-compliance}

<!-- Use this section description exactly as worded. -->
<!-- If there is a task that is the customer's responsibility and you have associated docs for how a customer completes that task, link to it from the Your responsibilities column. -->

Security and regulation compliance includes tasks such as security controls implementation and compliance certification.

| Task | {{site.data.keyword.IBM_notm}} responsibilities | Your responsibilities |
|------|-------------------------------------------------|-----------------------|
| Maintain currency with the compliance framework | * Ensure that the deployable architecture stays current with the {{site.data.keyword.framework-fs_notm}}.   \n * Provide default configuration that passes applicable compliance goals for an {{site.data.keyword.framework-fs_notm}} profile version, unless otherwise indicated. For more information, see [Change log: {{site.data.keyword.cloud_notm}} for Financial Services](/docs/security-compliance?topic=security-compliance-fs-change-log). | |
| Apply prerequisites | | Apply prerequisites as documented in the deployable architecture.|
| Verify configuration changes | | Understand the effects on the security and compliance posture of any user-initated changes to the default configuration. Run {{site.data.keyword.compliance_long}} checks if needed to ensure that the deployable architecture remains in compliance. |
{: row-headers}
{: caption="Table 4. Responsibilites for security and regulation compliance" caption-side="bottom"}
{: summary="The rows are read from left to right. The first column describes the task that the customer or IBM might be responsible for. The second column describes {{site.data.keyword.IBM_notm}} responsibilities for that task. The third column describes your responsibilities as the customer for that task."}

## Disaster recovery
{: #vpc-disaster-recovery}

<!-- Use this section description exactly as worded. -->
<!-- If there is a task that is the customer's responsibility and you have associated docs for how a customer completes that task, link to it from the Your responsibilities column. -->

Disaster recovery includes tasks such as providing dependencies on disaster recovery sites, provision disaster recovery environments, data and configuration backup, replicating data and configuration to the disaster recovery environment, and failover on disaster events.

The deployable architecture does not identify specific responsibilities in this area. For more information, see [Responsibilities for operating services in your deployment](/docs/framework-financial-services?topic=framework-financial-services-shared-responsibilities) in {{site.data.keyword.framework-fs_notm}}.
