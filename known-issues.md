---

copyright:
   years: 2023
lastupdated: "2023-10-03"

keywords:

subcollection: secure-infrastructure-vpc

---

{{site.data.keyword.attribute-definition-list}}

# Known issues with landing zone deployable architectures
{: #known-issues}

## Security and compliance tab displays all controls
{: #ki-scc-fscloud}

The "Security &amp; compliance" tab in the catalog detail pages of all landing zone deployable architectures lists all the controls that are defined for the IBM Cloud for Financial Services profile. The list is meant to include only the controls that the deployable architecture claims. Because all controls for the profile are listed, resources might fail the scan on controls that are not claimed by the deployable architecture.

## Some controls fail in Red Hat landing zone deployable architectures
{: #ki-ocp-version-fail}

Scans of the Red Hat OpenShift Container Platform on VPC landing zone deployable architecture can fail on the rule "Check whether OpenShift version is up-to-date". You see the failure with Red Hat OpenShift Container Platform version 4.12 after you deploy the deployable architecture and then run a scan in {{site.data.keyword.compliance_short}}. The following controls are part of the failure:

- CM-8 (3) - Automated Unauthorized Component Detection
- SI-2 (2) - Automated Flaw Remediation Status

Version 4.12 is a supported version. You can ignore this failure.

## VSI on VPC landing zone QuickStart variation fails projects validation
{: #ki-vsiqs-validation-fail}

With the QuickStart variation of the VSI on VPC landing zone that you deploy to {{site.data.keyword.cloud_notm}} projects, the configuration fails validation during the the Code Risk Analyzer scan. The QuickStart variation is designed to deploy quickly for demonstration and development and doesn't claim any controls for the scan.

## Service ID API keys and {{site.data.keyword.redhat_openshift_notm}}
{: #ki-svc-key-rhos}

Service ID API keys are not supported for the {{site.data.keyword.redhat_openshift_notm}} Container Platform on VPC landing zone deployable architecture. Use an {{site.data.keyword.cloud_notm}} [API key](https://cloud.ibm.com/docs/account?topic=account-userapikey#create_user_key). For more information, see [Ensuring that the API key or infrastructure credentials owner has the correct permissions](/docs/openshift?topic=openshift-access-creds#owner_permissions) in the {{site.data.keyword.redhat_openshift_notm}} Cloud Docs.

## Authentication with trusted profiles not supported in some landing zones
{: #ki-trusted-profile}

When you deploy a deployable architecture with {{site.data.keyword.cloud_notm}} projects, you select an authentication method to deploy your configuration.

Currently, the {{site.data.keyword.redhat_openshift_notm}} Container Platform on VPC landing zone supports authenticating only with an API key and not a service ID API key (see previous known issue). Trusted profile is not supported as an authentication method for this landing zone.

## Supported {{site.data.keyword.compliance_short}} rules
{: #ki-scc-goals}

The deployable architectures support a set of security controls, and those controls are listed with the deployable architecture in the {{site.data.keyword.cloud_notm}} catalog.

If you use {{site.data.keyword.compliance_short}} or the Code Risk Analyzer plug-in for {{site.data.keyword.cloud_notm}} to scan your deployed resources against another security profile, you might see failures in the results.

## Unsupported attribute error after interrupted apply or destroy
{: #ki-unsupported-attribute}

If a Terraform `apply` or `destroy` operation is interrupted, you might see an "Unsupported attribute" error at the next Terraform operation. Typically, this error occurs when a `destroy` operation is cancelled or has an unexpected error.

```text
Error: Unsupported attribute
   on .terraform/modules/landing_zone/dynamic_values/config_modules/vsi/vsi.tf line 20, in module "vsi_subnets":
   20:   subnet_zone_list = var.vpc_modules[each.value.vpc_name].subnet_zone_list
     ├────────────────
     │ each.value.vpc_name is "management"
     │ var.vpc_modules is object with 3 attributes

This object does not have an attribute named "subnet_zone_list".
```
{: screen}

The error occurs because the interrupted operation leaves key resources in a partial deployment state and other resources are configured to use these missing components.

### Workaround
{: #ki-unsupported-workaround}

To complete the `destroy` operation that is stuck in this state, try to decouple the resources that depend on the missing components. To decouple the resources, use the `override` feature, for example by setting the `override_json_string` input variable.

- VSI on VPC landing zone deployable architectures that are missing their VPC components: `override_json_string` = `'{"vsi": []}'`
- Red Hat OpenShift Container Platform on VPC landing zone deployable architectures:`override_json_string` = `'{"clusters": []}'`
