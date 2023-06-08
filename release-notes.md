---

copyright:
  years: 2023
lastupdated: "2023-06-08"

keywords:

subcollection: secure-infrastructure-vpc

content-type: release-note

---

{{site.data.keyword.attribute-definition-list}}

# Release notes for the landing zone deployable architecture
{: #secure-infrastructure-vpc-relnotes}

Use these release notes to learn about the latest updates to the landing zone deployable architectures: VPC landing zone, VSI on VPC landing zone, and Red Hat OpenShift Container Platform on VPC landing zone. The entries grouped by date.
{: shortdesc}

## June 2023
{: #landing-zone-2023-06}

### 01 June 2023
{: #secure-infrastructure-vpc-june0123}
{: release-note}

Version 4.0.0 of the landing zone deployable architectures available
:   Version 4.0.0 of the landing zone deployable architectures is available in the {{site.data.keyword.cloud_notm}} [catalog](/catalog#reference_architecture){: external}.

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

    For more information about implementing the Terraform logic, see the [release notes](https://github.com/terraform-ibm-modules/terraform-ibm-landing-zone/releases/tag/4.0.0) on GitHub.

## May 2023
{: #landing-zone-2023-05}

### 18 May 2023
{: #secure-infrastructure-vpc-may1823}
{: release-note}

Version 3.8.3 of the landing zone deployable architectures available
:   Version 3.8.3 of the landing zone deployable architectures is available in the {{site.data.keyword.cloud_notm}} [catalog](/catalog#reference_architecture){: external}. This version fixes the issue that prevented deletion of the SSH key in the VSI on VPC landing zone. For other changes in the release, see the [release notes](https://github.com/terraform-ibm-modules/terraform-ibm-landing-zone/releases/tag/v3.8.3){: external} on GitHub.

### 04 May 2023
{: #secure-infrastructure-vpc-may0523}
{: release-note}

Version 3.6.4 of the landing zone deployable architectures available
:   Version 3.6.4 of the landing zone deployable architectures is available in the {{site.data.keyword.cloud_notm}} catalog. This version includes updates to several variable descriptions and support for {{site.data.keyword.compliance_long}} v2 rules.

## 17 April 2023
{: #secure-infrastructure-vpc-apr1723}
{: release-note}

Introducing the landing zone deployable architectures
:   Three VPC landing zone deployable architectures are released: VPC landing zone, VSI on VPC landing zone, and Red Hat OpenShift Container Platform on VPC landing zone. You can use the deployable architectures to create a secure and customizable Virtual Private Cloud (VPC) environment. These [deployable architectures](#x10293733){: term} are based on the {{site.data.keyword.cloud_notm}} for Financial Services reference architecture. For more information about using deployable architectures with projects, see the blog posts [Projects and Cost Estimation: How IBM Cloud is Revolutionizing Complex Workloads for Enterprises](https://www.ibm.com/cloud/blog/announcements/projects-and-cost-estimation) and [Turn Your Terraform Templates into Deployable Architectures](https://www.ibm.com/cloud/blog/turn-your-terraform-templates-into-deployable-architectures).
