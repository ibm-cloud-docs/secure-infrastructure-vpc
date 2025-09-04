---

copyright:
  years: 2023, 2025
lastupdated: "2025-09-04"

keywords:

subcollection: secure-infrastructure-vpc

content-type: release-note

---

{{site.data.keyword.attribute-definition-list}}

# Release notes for the landing zone deployable architecture
{: #secure-infrastructure-vpc-relnotes}

Use these release notes to learn about the latest updates to the landing zone deployable architectures: Cloud foundation for VPC, VSI on VPC landing zone, and {{site.data.keyword.redhat_openshift_notm}} Container Platform on VPC landing zone. The entries are grouped by date.
{: shortdesc}

## September 2025
{: #landing-zone-2025-09}

### 4 September 2025
{: #secure-infrastructure-vpc-sep-0425}
{: release-note}

:   New versions of the Cloud foundation for VPC deployable architectures are released in the {{site.data.keyword.cloud_notm}} [catalog](/catalog#deployable_architecture){: external}.

    The name of the VPC deployable architectures has been renamed from "VPC landing zone" to "Cloud foundation for VPC"
    {: important}

    - Version 8.5.11 of the `Standard - Financial Services edition` (previously named `Standard`) variation is available.
        - The IBM Terraform provider version is now locked to 1.81.1
    - Version 8.2.0 of the new `Standard - Integrated setup with configurable services` variation is available.
        - Ideal for users who want flexibility with a reliable starting point.
        - Offers full control over architecture parameters, with well-chosen defaults that enable a functional Virtual Private Cloud (VPC) environment and integrated IBM Cloud services without requiring manual adjustments.

Version 8.5.0 of the landing zone deployable architectures is available
:   All landing zone deployable architectures are released at version 8.5.0 in the {{site.data.keyword.cloud_notm}} [catalog](/catalog#deployable_architecture){: external}.

    - The default virtual server image is updated to `ibm-ubuntu-24-04-2-minimal-amd64-5`. To avoid downtime and losing data, the image is not changed when you update to version 8.5.0. Update the image outside of the Terraform code.
    - The IBM Terraform provider version is now locked to 1.80.0.
    - The initial version of {{site.data.keyword.redhat_openshift_notm}} is now set to 4.18. Versions 4.14, 4.15, 4.16 and 4.17 are also supported. To avoid downtime and losing data, the cluster version is not changed when you update your deployable architecture. Update the cluster outside of the Terraform code. The default operating system for the worker nodes is now Red Hat Enterprise Linux CoreOS (RHCOS).


## August 2025
{: #landing-zone-2025-08}

### 13 August 2025
{: #secure-infrastructure-vpc-aug-1325}
{: release-note}

Version 8.5.0 of the landing zone deployable architectures is available
:   All landing zone deployable architectures are released at version 8.5.0 in the {{site.data.keyword.cloud_notm}} [catalog](/catalog#deployable_architecture){: external}.

    - The default virtual server image is updated to `ibm-ubuntu-24-04-2-minimal-amd64-5`. To avoid downtime and losing data, the image is not changed when you update to version 8.5.0. Update the image outside of the Terraform code.
    - The IBM Terraform provider version is now locked to 1.80.0.
    - The initial version of {{site.data.keyword.redhat_openshift_notm}} is now set to 4.18. Versions 4.14, 4.15, 4.16 and 4.17 are also supported. To avoid downtime and losing data, the cluster version is not changed when you update your deployable architecture. Update the cluster outside of the Terraform code. The default operating system for the worker nodes is now Red Hat Enterprise Linux CoreOS (RHCOS).

## June 2025
{: #landing-zone-2025-06}

### 10 June 2025
{: #secure-infrastructure-vpc-jun-1025}
{: release-note}

Version 8.2.0 of the landing zone deployable architectures is available
:   All landing zone deployable architectures are released at version 8.2.0 in the {{site.data.keyword.cloud_notm}} [catalog](/catalog#deployable_architecture){: external}.

    If you are upgrading from an older version of the VSI on VPC landing zone deployable architecture, the upgrade is going to destroy and re-create all virtual network interfaces and virtual service instances. This is because there was a bug where they were incorrectly being created in the Default resource group. The upgrade will now re-create them in the correct resource group. Ensure to plan accordingly before upgrading.
    {: important}

    - Added support for Atracker regions `in-che`, `jp-osa`, `jp-tok`, `br-sao`, and `ca-tor`
    - Added the ability to allow IP spoofing on the primary network interface by expsoing new boolean input `allow_ip_spoof` in the VSI on VPC landing zone deployable architecture.
    - Fixed a bug where the virtual network interface (VNI) was always getting created in the Default resource group (see above important note on breaking change).
    - Updated the scope of the KMS auth policy to now be scoped to the exact key. When upgrading from a previous version, the auth policy will be re-created, however the new policy will be created before the old one is destroyed so there will be no interruption to every day services.
    - The logic for VPC naming has been refactored so that there will be no `-vpc` suffix in the VPC name anymore.
    - The default virtual server image is updated to `ibm-ubuntu-24-04-6-minimal-amd64-1`. To avoid downtime and losing data, the image is not changed when you update to version 8.2.0. Update the image outside of the Terraform code.
    - The IBM terraform provider has been updated to version 1.79.0.

## February 2025
{: #landing-zone-2025-02}

### 21 February 2025
{: #secure-infrastructure-vpc-feb-2125}
{: release-note}

Version 7.2.2 of the landing zone deployable architectures is available
:   All landing zone deployable architectures are released at version 7.2.2 in the {{site.data.keyword.cloud_notm}} [catalog](/catalog#deployable_architecture){: external}.

    If you are upgrading from an older version, ensure to upgrade to version 6.8.1 before upgrading to this version. If you already selected this version from the IBM projects UI, and need to change it back to 6.8.1, follow these steps: [How do I choose an older DA version after selecting a later one in Projects config UI?](/docs/secure-infrastructure-vpc?topic=secure-infrastructure-vpc-ts-da-version-projects).
    {: important}

    - Update to the service to service IAM authorization policies to comply with the principle of least privilege (PoLP):
        - All of the Key Management Service (KMS) related service to service IAM authorization policies that are created by landing zone have been updated so that they are now scoped to the exact root key.
        - All of the Cloud Object Storage (COS) related service to service IAM authorization policies that are created by landing zone have been updated so that they are now scoped to the exact bucket name.
        - Plan accordingly if upgrading from an older version as these updates will cause current IAM authorization policies to be destroyed and recreated due to the nature of the refactoring.
    - Removed the logic that creates a virtual private endpoint (VPE) for Cloud Object Storage since it was clashing with the one created by VPC clusters and breaking cluster communication from worker nodes to the Cloud Object Storage direct endpoint. When upgrading from an older version, you will see the expected destroy of the virtual private endpoint for Cloud Object Storage and its associated reserved IP.
    - Updated configuration to use the new [ibm_cos_bucket_lifecycle_configuration](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/cos_bucket_lifecycle_configuration) for managing lifecycle configuration for a Cloud Object Storage bucket since the legacy approach is now deprecated. Migrating to the new schema addresses the limitations where one cannot create a rule with same rule id that has transition, expiration and abort_incomplete simultaneously, and not being able to create multiple abort_incomplete_multipart_upload rules.
    - The IBM terraform provider has been updated to version 1.75.2.

## January 2025
{: #landing-zone-2025-01}

### 31 January 2025
{: #secure-infrastructure-vpc-jan-3125}
{: release-note}

Version 6.8.1 of the landing zone deployable architectures is available
:   All landing zone deployable architectures are released at version 6.8.1 in the {{site.data.keyword.cloud_notm}} [catalog](/catalog#deployable_architecture){: external}.

    - This version is specifically designed for consumers who are still running on a 5.x.x version of the VSI on VPC landing zone or {{site.data.keyword.redhat_openshift_notm}} Container Platform on VPC landing zone deployable architectures and wish to upgrade to v6.x.x. When upgrading to 6.8.1, migration automation will be run automatically in IBM Cloud® projects that will prevent resources from being re-created. See [Troubleshooting issues when upgrading to version 6.8.1](/docs/secure-infrastructure-vpc?topic=secure-infrastructure-vpc-ts-migration) for further information.

    The VSI on VPC landing zone deployable architecture will now provision instances with the next gen [virtual network interface](https://cloud.ibm.com/docs/vpc?topic=vpc-vni-about) capabilities. There is no supported upgrade path to migrate existing virtual server instances on the legacy instance network interface to the new next gen virtual network interface, meaning if you are updating from a version 5.x.x version of the deployable architecture, you will see several resources identified for a destroy and re-create. If you want to remain on the legacy instance network interface, you can set the `use_legacy_network_interface` input to true before upgrading, and there should be no disruption to any of the resources you may already have deployed.
    {: important}

    - Support for {{site.data.keyword.redhat_openshift_notm}} version 4.17 has been added. Support for 4.12 and 4.13 have been removed. Versions 4.14, 4.15 and 4.16 are also supported. Version 4.16 is still the default.

### 16 January 2025
{: #secure-infrastructure-vpc-jan-1625}
{: release-note}

Version 6.7.0 of the landing zone deployable architectures is available
:   All landing zone deployable architectures are released at version 6.7.0 in the {{site.data.keyword.cloud_notm}} [catalog](/catalog#deployable_architecture){: external}.

    - VSI on VPC landing zone:
        - The default virtual server image is updated to `ibm-ubuntu-24-04-6-minimal-amd64-2`. To avoid downtime and losing data, the image is not changed when you update to version 6.7.0. Update the image outside of the Terraform code.
        - Support added for specifing optional [user data](https://cloud.ibm.com/docs/vpc?topic=vpc-user-data) that automatically performs common configuration tasks or runs scripts on the provisioned VSIs. For more information about how to use this feature, see [Adding user data to your VSI on VPC landing zone deployable architecture](/docs/secure-infrastructure-vpc?topic=secure-infrastructure-vpc-user-data).
    - Fixed a bug that was introduced in version 6.6.0 where an [Invalid index](/docs/secure-infrastructure-vpc?topic=secure-infrastructure-vpc-known-issues#ki-invlid-index-error) error was thrown for advanced users who have set `override = true` or passing a value for `override_json_string`.
    - The IBM terraform provider has been updated to version 1.74.0.

## December 2024
{: #landing-zone-2024-12}

### 12 December 2024
{: #secure-infrastructure-vpc-dec-1224}
{: release-note}

Version 6.6.0 of the landing zone deployable architectures is available
:   All landing zone deployable architectures are released at version 6.6.0 in the {{site.data.keyword.cloud_notm}} [catalog](/catalog#deployable_architecture){: external}.

    - The VSI on VPC landing zone deployable architecture will now provision instances with the next gen [virtual network interface](https://cloud.ibm.com/docs/vpc?topic=vpc-vni-about) capabilities.

        There is no supported upgrade path to migrate existing virtual server instances on the legacy instance network interface to the new next gen virtual network interface, meaning if you are updating from a previous version of the deployable architecture, you will see several resources identified for a destroy and re-create. If you want to remain on the legacy instance network interface, you can set the `use_legacy_network_interface` input to true before upgrading, and there should be no disruption to any of the resources you may already have deployed.
        {: important}

    - A fix was added to the "Existing VPC" variation of the VSI on VPC landing zone deployable architecture that ensures virtual server instances are only deployed in the correct subnets. Previously instances were created in every subnet, including ones that were not designed for virtual server instances. If upgrading from a previous version, the plan will identify for those virtual server instances to be destroyed.
    - The initial version of {{site.data.keyword.redhat_openshift_notm}} is now set to 4.16. Versions 4.12, 4.13, and 4.14 are also supported. To avoid downtime and losing data, the cluster version is not changed when you update your deployable architecture. Update the cluster outside of the Terraform code.
    - The IBM terraform provider has been updated to version 1.71.3.
    - The automation of adding client-to-site VPN is now available. The client-to-site VPN automation is ideal for scenarios where remote consumers need secure access to your private VPC network. In the catalog, there's a new Deployment Architecture (DA)called Cloud Automation for Client to Site VPN that makes it easier to extend your existing landing zone. It helps streamline the connection process, reducing setup time and minimizing the chances of errors.

## October 2024
{: #landing-zone-2024-10}

### 24 October 2024
{: #secure-infrastructure-vpc-oct-2424}
{: release-note}

Version 6.2.1 of the landing zone deployable architectures is available
:   All landing zone deployable architectures are released at version 6.2.1 in the {{site.data.keyword.cloud_notm}} [catalog](/catalog#deployable_architecture){: external}.

    - Controls in the {{site.data.keyword.compliance_long}} Framework for Financial Services profile version 1.7.0 that pass validation are now displayed.
    - The IBM Terraform provider version is now locked to 1.70.1.
    - VSI on VPC landing zone:
        - The default virtual server image is updated to `ibm-ubuntu-24-04-6-minimal-amd64-1`. To avoid downtime and losing data, the image is not changed when you update to version 6.2.1. Update the image outside of the Terraform code.
    - {{site.data.keyword.redhat_openshift_notm}} Container Platform on VPC landing zone:
        - Support for {{site.data.keyword.redhat_openshift_notm}} version 4.16 has been added. Versions 4.12, 4.13, 4.14 and 4.15 are also supported. Version 4.15 is still the default.
        - The `operating_system` input is now a required input. Valid values are `REDHAT_8_64` or `RHCOS`. By default, the input is set to `REDHAT_8_64`. If you are using the `override_json_string` input, this will need to be updated to include a value for `operating_system` if there is not currently one set. Ensure to also include it for any worker pools being added too. For example:
        ```json
        "clusters": [
          {
            "cos_name": "cos",
            "entitlement": "cloud_pak",
            "kube_type": "openshift",
            "kube_version": "default",
            "machine_type": "bx2.16x64",
            "name": "management-cluster",
            "resource_group": "slz-management-rg",
            "disable_outbound_traffic_protection": false,
            "cluster_force_delete_storage": false,
            "operating_system": "REDHAT_8_64",
            "kms_wait_for_apply": true,
            "kms_config": {
              "crk_name": "slz-roks-key",
              "private_endpoint": true
            },
            "subnet_names": [
              "vsi-zone-1",
              "vsi-zone-2",
              "vsi-zone-3"
            ],
            "vpc_name": "management",
            "worker_pools": [
              {
                "entitlement": "cloud_pak",
                "flavor": "bx2.16x64",
                "name": "logging-worker-pool",
                "subnet_names": [
                  "vsi-zone-1",
                  "vsi-zone-2",
                  "vsi-zone-3"
                ],
                "vpc_name": "management",
                "operating_system": "REDHAT_8_64",
                "workers_per_subnet": 2
              }
            ],
            "workers_per_subnet": 2
          }
        ]
        ```

## September 2024
{: #landing-zone-2024-09}

### 26 September 2024
{: #secure-infrastructure-vpc-sep-2624}
{: release-note}

Version 6.0.1 of the landing zone deployable architectures is available
:   All landing zone deployable architectures are released at version 6.0.1 in the {{site.data.keyword.cloud_notm}} [catalog](/catalog#deployable_architecture){: external}.

    - {{site.data.keyword.redhat_openshift_notm}} Container Platform on VPC landing zone:
        - A fix was added to address an issue where secondary storage was not being provisioned.
        - A fix was added to address and issue where the `workload_cluster_name` and `management_cluster_name` outputs were missing.

### 20 September 2024
{: #secure-infrastructure-vpc-sep-2024}
{: release-note}

Version 6.0.0 of the landing zone deployable architectures is available
:   All landing zone deployable architectures are released at version 6.0.0 in the {{site.data.keyword.cloud_notm}} [catalog](/catalog#deployable_architecture){: external}.

    - **BREAKING CHANGE:** VSI on VPC landing zone and {{site.data.keyword.redhat_openshift_notm}} Container Platform on VPC landing zone:
        - When you upgrade to version 6.0.0, you might see some of your infrastructure marked for deletion and re-creation. To prevent this from occurring, you should upgrade to version 6.8.0 directly before upgrading to any later version as this version has some migration logic in them that will prevent the re-creation of resources.
        - VPC landing zone deployable architecture is not affected.

    - Support added to pass an existing Context-based restriction (CBR) zone ID to allow all Virtual Private Clouds created to be added to the zone.
    - The IBM Terraform provider version is now locked to 1.69.2.
    - The Hashicorp external Terraform provider version is now locked to 2.3.4.
    - The VSI on VPC landing zone:
        - The `landing-zone-vsi` submodule is updated from 3.3.0 to 4.2.0. In this version, the naming convention for Virtual Server Instances has changed to be `prefix- + the last 4 digits of the subnet ID + a sequential number for each subnet`. For example, `prefix-3ad7-001`.
    - {{site.data.keyword.redhat_openshift_notm}} Container Platform on VPC landing zone:
        - Refactored the logic to use the [base-ocp-vpc](https://registry.terraform.io/modules/terraform-ibm-modules/base-ocp-vpc/ibm/latest) module to create {{site.data.keyword.redhat_openshift_notm}} Container Platform clusters.
        - This module has some extra functionality which requires the runtime to have access to IBM Cloud private endpoints.
        - Support added to allow you to specify a Virtual Private Cloud (VPC) that you do not wish to create a cluster in. By default a cluster will be created in all of the VPCs specified in the `vpcs` input. Use new input `ignore_vpcs_for_cluster_deployment` to pass a list of VPCs to ignore.

### 10 September 2024
{: #secure-infrastructure-vpc-sep-1024}
{: release-note}

Version 5.31.2 of the landing zone deployable architectures is available
:   All landing zone deployable architectures are released at version 5.31.2 in the {{site.data.keyword.cloud_notm}} [catalog](/catalog#deployable_architecture){: external}.

    - The default virtual server image is updated to `ibm-ubuntu-24-04-minimal-amd64-4`. To avoid downtime and losing data, the image is not changed when you update to version 5.31.2. Update the image outside of the Terraform code.
    - The IBM Terraform provider version is now locked to 1.69.0.
    - {{site.data.keyword.redhat_openshift_notm}} Container Platform on VPC landing zone:
        - The following output variables are now included for both the standard and QuickStart variations:
            - `workload_cluster_ingress_hostname`
            - `management_cluster_ingress_hostname`
            - `workload_cluster_private_service_endpoint_url`
            - `management_cluster_private_service_endpoint_url`
            - `workload_cluster_public_service_endpoint_url`
            - `management_cluster_public_service_endpoint_url`
            - `workload_cluster_console_url`
            - `management_cluster_console_url`
        - The following output variables are now included with the standard variation: `workload_cluster_name` and `management_cluster_name`.

## August 2024
{: #landing-zone-2024-08}

### 15 August 2024
{: #secure-infrastructure-vpc-aug-1524}
{: release-note}

Version 5.29.0 of the landing zone deployable architectures is available
:   All landing zone deployable architectures are released at version 5.29.0 in the {{site.data.keyword.cloud_notm}} [catalog](/catalog#deployable_architecture){: external}.

    - The `landing-zone-vpc` submodule is updated from 7.18.3 to 7.19.0.
    - A fix is included to make sure that `resource_group_names` and `resource_group_data` outputs include the value of the `prefix` input variable.
    - {{site.data.keyword.redhat_openshift_notm}} Container Platform on VPC landing zone:
        - A new `operating_system` input variable is added to specify the {{site.data.keyword.redhat_openshift_notm}} version of the cluster workers. The current version is the default. For more information, see [Available {{site.data.keyword.redhat_openshift_notm}} versions](/docs/openshift?topic=openshift-openshift_versions#openshift_versions_available).
        - The following optional input variables are now available to use existing resources for {{site.data.keyword.cos_full_notm}} and {{site.data.keyword.keymanagementservicelong_notm}} or {{site.data.keyword.hscrypto}}:
            - `existing_kms_instance_name`
            - `existing_kms_resource_group`
            - `existing_kms_endpoint_type`
            - `existing_cos_instance_name`
            - `existing_cos_resource_group`
            - `existing_cos_endpoint_type`
            - `use_existing_cos_for_vpc_flowlogs`
            - `use_existing_cos_for_atracker`

## July 2024
{: #landing-zone-2024-07}

### 15 July 2024
{: #secure-infrastructure-vpc-jul-1524}
{: release-note}

Version 5.25.1 of the landing zone deployable architectures is available
:   All landing zone deployable architectures are released at version 5.25.1 in the {{site.data.keyword.cloud_notm}} [catalog](/catalog#deployable_architecture){: external}.

    - The `landing-zone-vpc` submodule is updated from 7.18.2 to 7.18.3.
    - The default virtual server image is updated to `ibm-ubuntu-24-04-minimal-amd64-2`. To avoid downtime and losing data, the image is not changed when you update to version 5.24.5. Update the image outside of the Terraform code.
    - {{site.data.keyword.redhat_openshift_notm}} Container Platform on VPC landing zone:
        - A new `kms_wait_for_apply` input variable is added. The variable forces the code to wait until the key management service is applied to the cluster master, is ready, and deployed. The default value is `true`.
        - Added a fix to prevent an error with the new `ibm-storage-operator` add-on, which is installed by default on {{site.data.keyword.redhat_openshift_notm}} version 4.15.

## June 2024
{: #landing-zone-2024-06}

### 12 June 2024
{: #secure-infrastructure-vpc-jun-1224}
{: release-note}

Version 5.24.5 of the landing zone deployable architectures is available
:   All landing zone deployable architectures are released at version 5.24.5 in the {{site.data.keyword.cloud_notm}} [catalog](/catalog#deployable_architecture){: external}.

    - A graphical tool now exists to help you create the `override_json_string` optional input variable and customize your deployable architectures. Use the [landing zone configuration tool](https://terraform-ibm-modules.github.io/landing-zone-config-tool/#/home){: external} to customize aspects of your landing zone deployable architecture, including resource groups, Object Storage, key management, networking, private endpoints, and VPN gateways.
    - The default virtual server image is updated to `ibm-ubuntu-24-04-minimal-amd64-1`. To avoid downtime and losing data, the image is not changed when you update to version 5.24.5. Update the image outside of the Terraform code.
    - The IBM Terraform provider version is now locked to 1.66.0.
    - The `landing-zone-vpc` submodule is updated from 7.18.0 to 7.18.2. For more information about changes in the submodule, see VSI on VPC [release v7.18.2](https://github.com/terraform-ibm-modules/terraform-ibm-landing-zone-vpc/releases/tag/v7.18.2){: external} in GitHub.
    - The VSI on VPC landing zone:
        - The `landing-zone-vsi` submodule is updated from 3.2.4 to 3.3.0. For more information about changes in the submodule, see VSI on VPC [release v3.3.0](https://github.com/terraform-ibm-modules/terraform-ibm-landing-zone-vsi/releases/tag/v3.3.0){: external} in GitHub.
    - The {{site.data.keyword.redhat_openshift_notm}} Container Platform on VPC landing zone:
        - The initial version of {{site.data.keyword.redhat_openshift_notm}} is now set to 4.15. Versions 4.12, 4.13, and 4.14 are also supported. To avoid downtime and losing data, the cluster version is not changed when you update your deployable architecture. Update the cluster outside of the Terraform code.
        - A new `cluster_force_delete_storage` input variable is added. The variable specifies whether to force the deletion of persistent storage when the associated VPC cluster is deleted so that the cluster can't be recovered. The default value is `false` in the Standard variation and `true` in the QuickStart variation.

## May 2024
{: #landing-zone-2024-05}

### 1 May 2024
{: #secure-infrastructure-vpc-may-0124}
{: release-note}

Version 5.21.0 of the landing zone deployable architectures is available
:   All landing zone deployable architectures are released at version 5.21.0 in the {{site.data.keyword.cloud_notm}} [catalog](/catalog#deployable_architecture){: external}.

    - Controls in the {{site.data.keyword.compliance_long}} Framework for Financial Services profile version 1.6.0 that pass validation are now displayed.
    - The `landing-zone-vsi` submodule is updated from 3.2.3 to 3.2.4. For more information about changes in the submodule, see VSI on VPC [release v3.2.4](https://github.com/terraform-ibm-modules/terraform-ibm-landing-zone-vsi/releases/tag/v3.2.4){: external} in GitHub.
    - The default virtual server image is updated to `ibm-ubuntu-22-04-4-minimal-amd64-1`. To avoid downtime and losing data, the image is not changed when you update to version 5.21.0. Update the image outside of the Terraform code.
    - The QuickStart variation of the {{site.data.keyword.redhat_openshift_notm}} Container Platform on VPC landing zone:
        - A new `entitlement` input variable is added. The variable defaults to no entitlement and is applied only when the cluster is created.
        - The variation now includes more outputs. For more information, see the landing zone [release v5.21.0](https://github.com/terraform-ibm-modules/terraform-ibm-landing-zone/releases/tag/v5.21.0){: external} in GitHub.

## March 2024
{: #landing-zone-2024-03}

### 22 March 2024
{: #secure-infrastructure-vpc-mar-2224}
{: release-note}

Version 5.20.0 of the landing zone deployable architectures is available
:   Version 5.20.0 of the landing zone deployable architecture is available in the {{site.data.keyword.cloud_notm}} [catalog](/catalog#deployable_architecture){: external}.

    - The IBM Terraform provider version is now locked to 1.63.0.
    - The external provider version is now set to 2.3.3.
    - The {{site.data.keyword.redhat_openshift_notm}} Container Platform on VPC landing zone:
        - This version includes a new QuickStart variation. For more information, see the [reference architecture](/docs/secure-infrastructure-vpc?topic=secure-infrastructure-vpc-roks-ra-qs).
        - New variables `cluster_addons` and `manage_all_cluster_addons` are added to support the configuration of cluster add-ons.

### 1 March 2024
{: #secure-infrastructure-vpc-mar-0124}
{: release-note}

Version 5.17.2 of the landing zone deployable architectures is available
:   Version 5.17.2 of the landing zone deployable architecture is available in the {{site.data.keyword.cloud_notm}} [catalog](/catalog#deployable_architecture){: external}.

    - The deployable architecture is updated to allow all versions of Terraform 1.6. Versions 1.3.0 - 1.6.x are allowed. The `landing-zone-vpc` and `landing-zone-vsi` submodules are also updated to support these versions.
    - The IBM Terraform provider version is now locked to 1.62.0.

## February 2024
{: #landing-zone-2024-02}

### 6 February 2024
{: #secure-infrastructure-vpc-feb-0624}
{: release-note}

Version 5.14.0 of the landing zone deployable architectures is available
:   Version 5.14.0 of the landing zone deployable architecture is available in the {{site.data.keyword.cloud_notm}} [catalog](/catalog#deployable_architecture){: external}.

    - The `time_sleep.wait_for_authorization_policy` resource is destroyed when you upgrade to this version. This behavior is expected and does not affect your provisioned infrastructure.
    - The initial version of {{site.data.keyword.redhat_openshift_notm}} is now set to 4.14. Versions 4.12 and 4.13 are also supported. To avoid downtime and losing data, the cluster version is not changed when you update your deployable architecture. Update the cluster outside of the Terraform code.
    - A new `skip_all_s2s_auth_policies` variable is available to manage authorization policies outside of your deployable architecture. To keep names consistent, the `add_kms_block_storage_s2s` variable is renamed to `skip_kms_block_storage_s2s_auth_policy`.
    - The version now exposes the `secondary_storage` variable for the Kubernetes service from the IBM Terraform Provider. Use the variable to provision a secondary disk to your worker nodes.
    - A service-to-service authorization policy between Kubernetes and your KMS is now created when you provision a cluster. This change fixes an issue that the policy was not created by default.
    - A `service_endpoint` input variable supports whether access to {{site.data.keyword.keymanagementserviceshort}} is through a public or private-only endpoint. The default value is `public-and-private.`
    - Removed in this version:
        - Version 4.11 of {{site.data.keyword.redhat_openshift_notm}} is no longer supported.
        - The `update_all_workers` input variable is removed. The variable was meant to update the Kubernetes version of all workers, but the deployable architecture ignores the version.
    - You can now set an expiration rule for {{site.data.keyword.cos_full_notm}} buckets in an override. If you deploy with projects or {{site.data.keyword.bplong_notm}}, edit the `override_json_string` optional variable. For example, the following example adds a 30-day expiration in the `expire_rule` property:

        ```json
        "cos": [
          {
            "buckets": [
              {
                "endpoint_type": "public",
                "force_delete": true,
                "kms_key": "slz-atracker-key",
                "name": "atracker-bucket",
                "storage_class": "standard",
                "region_location": "us-south",
                "hard_quota": 0,
                "expire_rule": {
                  "rule_id": "a-bucket-expire-rule",
                  "enable": true,
                  "days": 30,
                  "prefix": "logs/"
                }
              }
            ]
          }
        ]
        ```

## December 2023
{: #landing-zone-2023-12}

### 15 December 2023
{: #secure-infrastructure-vpc-dec-1523}
{: release-note}

Version 5.3.1 of the landing zone deployable architectures available
:   Version 5.3.1 of the landing zone deployable architectures is available in the {{site.data.keyword.cloud_notm}} [catalog](/catalog#deployable_architecture){: external}.

    - The version includes a new extension variation that is called VSI on existing VPC landing zone. It adds a VSI in an existing VPC.

        With this variation, you extend either the VPC landing zone or the {{site.data.keyword.redhat_openshift_notm}} Container Platform on VPC landing zone. For more information, see [Adding a VSI to your VPC landing zone deployable architecture](/docs/secure-infrastructure-vpc?topic=secure-infrastructure-vpc-ext-with-vsi).
    - The IBM Terraform provider version is now locked to 1.60.0. This provider version fixes the known issue that the provider plug-in did not respond. For more information, see [issue 4898](https://github.com/IBM-Cloud/terraform-provider-ibm/issues/4898){: external}.
    - The default virtual server image is updated to `ibm-ubuntu-22-04-3-minimal-amd64-2`. To avoid downtime and losing data, the image is not changed when you update to version 5.3.1. Update the image outside of the Terraform code.
    - This version of the {{site.data.keyword.redhat_openshift_notm}} Container Platform on VPC landing zone fixes the issue where the value of the `wait_till` input variable is not used.

### 04 December 2023
{: #secure-infrastructure-vpc-dec-0423}
{: release-note}

Version 5.1.0 of the landing zone deployable architectures available
:   Version 5.1.0 of the landing zone deployable architectures is available in the {{site.data.keyword.cloud_notm}} [catalog](/catalog#deployable_architecture){: external}.

    - Backward-incompatible change for VSIs with floating IPs.
        - If your landing zone deployable architecture provisions a VSI, and you enabled a floating IP address for it, the IP addresses are deleted and re-created when you apply the changes in this version.
        - If you deployed with the default settings, only the [QuickStart variation](/docs/secure-infrastructure-vpc?topic=secure-infrastructure-vpc-vsi-ra-qs) of the VSI on VPC landing zone deployable architecture is affected because it provisions a floating IP address for use as a jump box. However, any deployable architecture in which you provisioned floating IP addresses are affected.
        -  The removal happens because the IP addresses were created in the wrong resource group (the default group). The IP addresses are re-created in the same resource group as the VSI.

        Plan for the change to make sure that anything using the IP addresses is not disrupted.
    - Other changes
        - Added support for for Madrid (`eu-es`) region.
        - Added support for configuring the idle connection timeout value for any VSI load balancers that are provisioned.
        - Removed support for VPN gateway connections. Current connections, if manually made, are not removed when you update to this version. However, if the connections were created in an override, they will be removed when you update to this version.

## November 2023
{: #landing-zone-2023-11}

### 02 November 2023
{: #secure-infrastructure-vpc-nov-0223}
{: release-note}

Version 4.13.3 of the landing zone deployable architectures available
:   Version 4.13.3 of the landing zone deployable architectures is available in the {{site.data.keyword.cloud_notm}} [catalog](/catalog#deployable_architecture){: external}.

    - The IBM Terraform provider version is now locked to 1.59.0.
    - The `landing-zone-vsi` submodule is updated from 2.6.0 to 2.12.1. For more information about changes in the submodule, see [issue 577](https://github.com/terraform-ibm-modules/terraform-ibm-landing-zone/pull/577) in GitHub.
    - The VSI on VPC landing zone and the {{site.data.keyword.redhat_openshift_notm}} Container Platform on VPC landing zone deployable architectures now include a `vpc_resource_list` output.
    - The {{site.data.keyword.redhat_openshift_notm}} Container Platform on VPC landing zone deployable architecture now includes a `cluster_data` output with information about the created clusters, including IDs and names of the clusters.
    - The initial version of the OpenShift cluster is now set to 4.13 by default. Versions 4.11 and 4.12 are also supported. To avoid downtime and losing data, the cluster version is not changed when you update your deployable architecture. Update the cluster outside of the Terraform code.

        Version 4.10 of {{site.data.keyword.redhat_openshift_notm}} is no longer supported.

## October 2023
{: #landing-zone-2023-10}

### 03 October 2023
{: #secure-infrastructure-vpc-oct0323}
{: release-note}

Version 4.12.3 of the landing zone deployable architectures available
:   Version 4.12.3 of the landing zone deployable architectures is available in the {{site.data.keyword.cloud_notm}} [catalog](/catalog#deployable_architecture){: external}.

    - For the `existing_ssh_key_name` variable, you can now select from a list of all keys in the account when you deploy with projects or {{site.data.keyword.bplong_notm}}.
    - Deployable architectures now use the {{site.data.keyword.cloud_notm}} Terraform provider resource `clean_default_sg_acl` to clean the default ACL and security group rules. The new resource replaces the `null_resource.clean_default_security_group[0]` and `null_resource.clean_default_acl[0]` resources. When you upgrade from v4.4.7, the null resources are destroyed. This behavior is expected and does not affect your provisioned infrastructure.
    - You can now attach existing access tags to resources that are provisioned by the deployable architecture in an override. If you deploy with projects or {{site.data.keyword.bplong_notm}}, edit the `override_json_string` optional variable as in the following example that adds a tag to the key management resources:

        ```json
        {
          "key_management": {
            "access_tags": ["tag-group:tag-name"]
          }
        }
        ```
        {: codeblock}

    - For deployable architectures with a transit gateway enabled, a new `transit_gateway_global` variable supports connecting to networks outside the associated region.
    - Key management changes:
        - You can now use key management keys that you create outside the deployable architecture or from different accounts by specifying the key CRN in the `existing_key_crn` field in an override. If you deploy with projects, edit the `override_json_string` optional variable. For more information, see https://github.com/terraform-ibm-modules/terraform-ibm-landing-zone/releases/tag/v4.9.0.
        - The following outputs are now included with key management resources: `key_management_name`, `key_management_crn`, `key_management_guid`, `key_rings`, and `key_map`.
    - Added placement group details to the outputs. A placement group provides flexibility with performance and high availability for {{site.data.keyword.hpc-cluster_short}} solutions.
    - Changes related to VSI on VPC landing zone:
        - The default virtual server image is updated to `ibm-ubuntu-22-04-3-minimal-amd64-1`. To avoid downtime and losing data, the image is not changed when you update to version 4.12.3. Update the image outside of the Terraform code.
        - You can't upgrade the QuickStart variation from v4.4.7 because of an issue with the provider configuration. Create another instance of the VSI on VPC landing zone QuickStart variation.
    - Changes related to {{site.data.keyword.redhat_openshift_notm}} Container Platform on VPC landing zone:
        - Added OpenShift Container Platform boot volume KMS encryption support. The boot volume is enabled for new clusters, but is not enabled when you upgrade because encryption can happen only during the initial provisioning.

## July 2023
{: #landing-zone-2023-07}

### 31 July 2023
{: #secure-infrastructure-vpc-july3123}
{: release-note}

Version 4.4.7 of the landing zone deployable architectures available
:   Version 4.4.7 of the landing zone deployable architecture is available in the {{site.data.keyword.cloud_notm}} [catalog](/catalog#deployable_architecture){: external}.

    - For {{site.data.keyword.redhat_openshift_notm}} Container Platform on VPC landing zone:
        - Changes to the version of the OpenShift cluster are now ignored. Make sure that you update your cluster version outside of the Terraform in the deployable architecture to prevent destructive changes to your infrastructure.
        - The initial version of the OpenShift cluster is now set to version 4.12 by default. Versions 4.11 and 4.10 are also supported.

### 06 July 2023
{: #secure-infrastructure-vpc-july0623}
{: release-note}

Version 4.4.1 of the landing zone deployable architectures available
:   Version 4.4.1 of the landing zone deployable architectures is available in the {{site.data.keyword.cloud_notm}} [catalog](/catalog#deployable_architecture){: external}.

    - {{site.data.keyword.cloud_notm}} {{site.data.keyword.hscrypto}} is now supported by the deployable architectures. {{site.data.keyword.hscrypto}} supports keep your own key (KYOK) features. The default encryption remains {{site.data.keyword.keymanagementserviceshort}}. For more information, see the [planning information](/docs/secure-infrastructure-vpc?topic=secure-infrastructure-vpc-plan#vpc-crypto-prereqs).
    - The IBM provider version is updated to 1.54.0, which fixes a known issue with failures when creating an authorization policy.
    - For the VSI on VPC landing zone, the default virtual server image is updated to `ibm-ubuntu-22-04-2-minimal-amd64-1` from `ibm-ubuntu-22-04-1-minimal-amd64-4`, which is deprecated. To avoid downtime and losing data, the image is not changed when you update to version 4.4.1.

   A known issue exists with validation when you deploy by using projects.

## June 2023
{: #landing-zone-2023-06}

### 01 June 2023
{: #secure-infrastructure-vpc-june0123}
{: release-note}

Version 4.0.0 of the landing zone deployable architectures available
:   Version 4.0.0 of the landing zone deployable architectures is available in the {{site.data.keyword.cloud_notm}} [catalog](/catalog#deployable_architecture){: external}.

    By default, the default VPC security group and ACLs are removed. Because the landing zone deployable architectures create security groups, the VPC defaults are not needed and might be more permissive than required.

    To prevent the default group and rules from being removed, specify your VPC configuration by using an override block. Add the  `clean_default_security_group` and `clean_default_acl` properties set to `false` to the block. For example, if you deploy with projects, edit the `override_json_string` optional variable as in the following example:

    ```json
    "vpcs": [
      {
        "prefix": "management",
        "clean_default_security_group": false,
        "clean_default_acl": false,
        . . .  # the rest of your VPC configuration
      }]
    ```
    {: codeblock}

    For more information about implementing the Terraform logic, see the [release notes](https://github.com/terraform-ibm-modules/terraform-ibm-landing-zone/releases/tag/v4.0.0) on GitHub.

## May 2023
{: #landing-zone-2023-05}

### 18 May 2023
{: #secure-infrastructure-vpc-may1823}
{: release-note}

Version 3.8.3 of the landing zone deployable architectures available
:   Version 3.8.3 of the landing zone deployable architectures is available in the {{site.data.keyword.cloud_notm}} [catalog](/catalog#deployable_architecture){: external}. This version fixes the issue that prevented deletion of the SSH key in the VSI on VPC landing zone. For other changes in the release, see the [release notes](https://github.com/terraform-ibm-modules/terraform-ibm-landing-zone/releases/tag/v3.8.3){: external} on GitHub.

### 04 May 2023
{: #secure-infrastructure-vpc-may0523}
{: release-note}

Version 3.6.4 of the landing zone deployable architectures available
:   Version 3.6.4 of the landing zone deployable architectures is available in the {{site.data.keyword.cloud_notm}} catalog. This version includes updates to several variable descriptions and support for {{site.data.keyword.compliance_long}} v2 rules.

## 17 April 2023
{: #secure-infrastructure-vpc-apr1723}
{: release-note}

Introducing the landing zone deployable architectures
:   Three VPC landing zone deployable architectures are released: VPC landing zone, VSI on VPC landing zone, and {{site.data.keyword.redhat_openshift_notm}} Container Platform on VPC landing zone. You can use the deployable architectures to create a secure and customizable Virtual Private Cloud (VPC) environment. These [deployable architectures](#x10293733){: term} are based on the {{site.data.keyword.cloud_notm}} for Financial Services reference architecture. For more information about using deployable architectures with projects, see the blog posts [Projects and Cost Estimation: How IBM Cloud is Revolutionizing Complex Workloads for Enterprises](https://www.ibm.com/blog/announcement/projects-and-cost-estimation/) and [Turn Your Terraform Templates into Deployable Architectures](https://www.ibm.com/blog/turn-your-terraform-templates-into-deployable-architectures/).
