---

copyright:
  years: 2023, 2024
lastupdated: "2024-07-22"

keywords:

subcollection: secure-infrastructure-vpc

---

{{site.data.keyword.attribute-definition-list}}

# Planning for the landing zone deployable architectures
{: #plan}

Before you begin the deployment of a landing zone deployable architecture, make sure that you understand and meet the prerequisites.
{: shortdesc}

## Confirm your {{site.data.keyword.cloud_notm}} settings
{: #vpc-cloud-prereqs}

Complete the following steps before you deploy the VPC landing zone deployable architecture.

1.  Confirm or set up an {{site.data.keyword.cloud_notm}} account:

    Make sure that you have an {{site.data.keyword.cloud_notm}} Pay-As-You-Go or Subscription account:

    - If you don't have an {{site.data.keyword.cloud_notm}} account, [create one](/docs/account?topic=account-account-getting-started).
    - If you have a Trial or Lite account, [upgrade your account](/docs/account?topic=account-upgrading-account).
1.  Configure your {{site.data.keyword.cloud_notm}} account:
    1.  Log in to [{{site.data.keyword.cloud_notm}}](https://cloud.ibm.com) with the {{site.data.keyword.ibmid}} you used to set up the account. This {{site.data.keyword.ibmid}} user is the account owner and has full IAM access.
    1.  [Complete the company profile](/docs/account?topic=account-contact-info) and contact information for the account. This profile is required to stay in compliance with {{site.data.keyword.cloud_notm}} Financial Services profile.
    1.  [Enable the Financial Services Validated option](/docs/account?topic=account-enabling-fs-validated) for your account.
    1.  Enable virtual routing and forwarding (VRF) and service endpoints by creating a support case. Follow the instructions in [enabling VRF and service endpoints](/docs/account?topic=account-vrf-service-endpoint&interface=ui#vrf).

## Set the IAM permissions
{: #vpc-iam-prereqs}

1.  Set up account access ({{site.data.keyword.iamshort}} (IAM)):
    1.  Create an {{site.data.keyword.cloud_notm}} [API key](/docs/account?topic=account-userapikey&interface=terraform#create_user_key-api-terra). The user who owns this key must have the Administrator role.

        Service ID API keys are not supported for the Red Hat OpenShift Container Platform on VPC landing zone deployable architecture.
        {: tip}

    1.  For compliance with {{site.data.keyword.framework-fs_notm}}: Require users in your account to use [multifactor authentication (MFA)](/docs/account?topic=account-account-getting-started#account-gs-mfa).
    1.  [Set up access groups](/docs/account?topic=account-account-getting-started#account-gs-accessgroups).

        User access to {{site.data.keyword.cloud_notm}} resources is controlled by using the access policies that are assigned to access groups. For {{site.data.keyword.cloud_notm}} Financial Services validation, do not assign direct IAM access to any {{site.data.keyword.cloud_notm}} resources.

        Select **All Identity and Access enabled services** when you assign access to the group.

### Verify access roles
{: #vpc-access-roles}

IAM access roles are required to install this deployable architecture and create all the required elements.

You need the following permissions for this deployable architecture:

- Create services from {{site.data.keyword.cloud_notm}} catalog.
- Create and modify {{site.data.keyword.vpc_short}} services, virtual server instances, networks, network prefixes, storage volumes, SSH keys, and security groups of this VPC.
- Create and modify {{site.data.keyword.cloud_notm}} direct links and {{site.data.keyword.tg_full_notm}}.
- Access existing {{site.data.keyword.cos_short}} services.

For information about configuring permissions, contact your {{site.data.keyword.cloud_notm}} account administrator.

### Access for {{site.data.keyword.cloud_notm}} projects
{: #access-projects}

You can use {{site.data.keyword.cloud_notm}} projects as a deployment option. Projects are designed with infrastructure as code and compliance in mind to help ensure that your projects are managed, secure, and always compliant. For more information, see [Learn about IaC deployments with projects](/docs/secure-enterprise?topic=secure-enterprise-understanding-projects).

You need the following access to create a project and create project tooling resources within the account. Make sure you have the following access:

- The Editor role on the Projects service.
- The Editor and Manager role on the {{site.data.keyword.bpshort}} service
- The Viewer role on the resource group for the project

For more information, see [Assigning users access to projects](/docs/secure-enterprise?topic=secure-enterprise-access-project).

## Create an SSH key
{: #vpc-ssh-key}

Make sure that you have an SSH key that you can use for authentication. This key is used to log in to all virtual server instances that you create. For more information about creating SSH keys, see [SSH keys](/docs/vpc?topic=vpc-ssh-keys).

## (Optional) Set up {{site.data.keyword.cloud_notm}} {{site.data.keyword.hscrypto}}
{: #vpc-crypto-prereqs}

For key management services, you can use {{site.data.keyword.cloud_notm}} {{site.data.keyword.hscrypto}} instead of {{site.data.keyword.cos_full_notm}}. {{site.data.keyword.hscrypto}} is a dedicated key management service and hardware security module based on {{site.data.keyword.cloud_notm}} that enables keep your own key (KYOK) features.

By using {{site.data.keyword.hscrypto}}, your deployable architecture satisfies the requirements for the following controls:

- [SC-13(0) - Cryptographic Protection](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-sc-13)
- [SC-28(0) - Protection of Information at Rest](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-sc-28)
- [SC-28(1) - Cryptographic Protection](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-sc-28.1)

For more information, see the [security information](/docs/framework-financial-services?topic=framework-financial-services-vpc-architecture-about#services-security-hpcs) in the VPC reference architecture for IBM Cloud for Financial Services.

It is not possible to update an existing deployable architecture from {{site.data.keyword.keymanagementserviceshort}} to {{site.data.keyword.hscrypto}}. You must create and deploy another deployable architecture.
{:restriction: .restriction}

### Provisioning and initializing the {{site.data.keyword.hscrypto}} service
{: #vpc-hpcs-setup}

Before you deploy this deployable architecture, you need an instance of the Hyper Protect Crypto Services service.

1.  You can provision {{site.data.keyword.hscrypto}} in one of two ways:

    - By using the [{{site.data.keyword.cloud_notm}} {{site.data.keyword.hscrypto}}](https://github.com/terraform-ibm-modules/terraform-ibm-hpcs){: external} Terraform module.
    - By creating and initializing an instance directly.

        1.  (Optional) [Create a resource group](/docs/account?topic=account-rgs&interface=ui) for your instance.
        1.  On the {{site.data.keyword.hscrypto}} [details page](https://cloud.ibm.com/catalog/services/hyper-protect-crypto-services), select a plan.
        1.  Complete the required details and click **Create**.

1.  Initialize {{site.data.keyword.hscrypto}}:

    - If you used the {{site.data.keyword.cloud_notm}} {{site.data.keyword.hscrypto}} module, follow the steps in the module [readme file](https://github.com/terraform-ibm-modules/terraform-ibm-hpcs#create-hyper-protect-crypto-services-instance){: external}.
    - If you created the instance directly, follow the steps in [Getting started with {{site.data.keyword.cloud_notm}} {{site.data.keyword.hscrypto}}](/docs/hs-crypto?topic=hs-crypto-get-started).

    For proof-of-technology environments, use the `auto-init` flag. For more information, see [Initializing service instances using recovery crypto units](/docs/hs-crypto?topic=hs-crypto-initialize-hsm-recovery-crypto-unit).

1.  When you configure your deployable architecture, specify the resource group in the `hs_crypto_resource_group` input variable and the instance name in the `hs_crypto_instance_name` variable. If you don't provide values for those variables, the default {{site.data.keyword.keymanagementserviceshort}} encryption is used.
