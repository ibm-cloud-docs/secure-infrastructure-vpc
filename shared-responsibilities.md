---

copyright:
  years: 2023
lastupdated: "2024-10-09"

keywords:

subcollection: secure-infrastructure-vpc

---

{{site.data.keyword.attribute-definition-list}}

# Understanding your responsibilities when you use the landing zone deployable architectures
{: #shared-resp}

Learn about the management responsibilities and terms and conditions that you have when you use one of the landing zone deployable architectures.
{: shortdesc}

- For more information about the responsibilities for you and for {{site.data.keyword.IBM}} when you use a deployable architecture, see [Understanding your responsibilities when you use deployable architectures](/docs/secure-enterprise?topic=secure-enterprise-responsibilities-deployable-architectures).
- For a high-level view of the service types in {{site.data.keyword.cloud}} and the breakdown of responsibilities between the customer and {{site.data.keyword.IBM_notm}} for each type, see [Shared responsibilities for using {{site.data.keyword.cloud}} products](/docs/overview?topic=overview-shared-responsibilities).
- For the overall terms of use, see [{{site.data.keyword.cloud_notm}} Terms and Notices](/docs/overview?topic=overview-terms).
- For more information about the shared responsibilities for each Financial Services Validated service, see [Responsibilities for operating services in your deployment](/docs/framework-financial-services?topic=framework-financial-services-shared-responsibilities) in {{site.data.keyword.framework-fs_full}}.

Review the following sections for the specific responsibilities for you and for {{site.data.keyword.IBM_notm}} when you use a landing zone deployable architecture.



## Incident and operations management
{: #vpc-incident-and-ops}

Incident and operations management includes tasks such as monitoring, event management, high availability, problem determination, recovery, and full state backup and recovery.

The landing zone deployable architectures do not identify specific responsibilities in this area.

## Change management
{: #vpc-change-management}

Change management includes tasks such as deployment, configuration, upgrades, patching, configuration changes, and deletion.

| Task | {{site.data.keyword.IBM_notm}} responsibilities | Your responsibilities |
|------|-------------------------------------------------|-----------------------|
| Keep deployed services and resources up to date | | Apply fixes and updates to the compute resources that are created from the deployable architecture. The following resources are not updated through the deployable architecture unless otherwise indicated.   \n * [Red Hat OpenShift clusters](/docs/openshift?topic=openshift-update)  \n * [Kubernetes lusters, worker nodes, and cluster components](/docs/containers?topic=containers-update) |
{: row-headers}
{: caption="Responsibilities for change management" caption-side="bottom"}
{: summary="The rows are read from left to right. The first column describes the task that the customer or IBM might be responsible for. The second column describes {{site.data.keyword.IBM_notm}} responsibilities for that task. The third column describes your responsibilities as the customer for that task."}


## Identity and access management
{: #vpc-iam-responsibilities}

Identity and access management includes tasks such as authentication, authorization, access control policies, and approving, granting, and revoking access.

| Task | {{site.data.keyword.IBM_notm}} responsibilities | Your responsibilities |
|------|-------------------------------------------------|-----------------------|
| Secure with least privilege | Document the minimal IAM access requirements to run the deployable architecture. |  |
| Manage secrets | | * Generate the necessary secrets (for example, IAM API keys, SSH keys) that are required for the deployable architecture.  \n * Manage generated secrets by following secure best practices. |
{: row-headers}
{: caption="Responsibilities for identity and access management" caption-side="bottom"}
{: summary="The rows are read from left to right. The first column describes the task that the customer or IBM might be responsible for. The second column describes {{site.data.keyword.IBM_notm}} responsibilities for that task. The third column describes your responsibilities as the customer for that task."}

## Security and regulation compliance
{: #vpc-security-compliance}

Security and regulation compliance includes tasks such as security controls implementation and compliance certification.

| Task | {{site.data.keyword.IBM_notm}} responsibilities | Your responsibilities |
|------|-------------------------------------------------|-----------------------|
| Meet security and compliance objectives | Provide a deployable architecture that complies with the set of controls that are defined with the release of the deployable architecture. The controls in the deployable architecture do not necessarily cover the complete profile for the {{site.data.keyword.framework-fs_notm}}, as shown in the [Available predefined profiles](/docs/security-compliance?topic=security-compliance-predefined-profiles).
| Verify configuration changes | | Understand the effects on the security and compliance posture of any user-initiated changes to the default configuration. Run {{site.data.keyword.compliance_long}} checks if needed to ensure that the deployable architecture remains in compliance. |
{: row-headers}
{: caption="Responsibilities for security and regulation compliance" caption-side="bottom"}
{: summary="The rows are read from left to right. The first column describes the task that the customer or IBM might be responsible for. The second column describes {{site.data.keyword.IBM_notm}} responsibilities for that task. The third column describes your responsibilities as the customer for that task."}

## Disaster recovery
{: #vpc-disaster-recovery}




Disaster recovery includes tasks such as providing dependencies on disaster recovery sites, provision disaster recovery environments, data and configuration backup, replicating data and configuration to the disaster recovery environment, and failover on disaster events.

The landing zone deployable architectures do not identify specific responsibilities in this area.
