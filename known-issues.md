---

copyright:
  years: 2023, 2024
lastupdated: "2024-04-23"

keywords:

subcollection: secure-infrastructure-vpc

---

{{site.data.keyword.attribute-definition-list}}

# Known issues with landing zone deployable architectures
{: #known-issues}

## QuickStart variations fail projects validation
{: #ki-vsiqs-validation-fail}

With the QuickStart variations of the landing zones (both VSI on VPC and Red Hat OpenShift Container Platform on VPC), the configuration fails validation during the the Code Risk Analyzer scan. QuickStart variations are designed to deploy quickly for demonstration and development and don't claim any controls for the scan.

## Service ID API keys and {{site.data.keyword.redhat_openshift_notm}}
{: #ki-svc-key-rhos}

Service ID API keys are not supported for the {{site.data.keyword.redhat_openshift_notm}} Container Platform on VPC landing zone deployable architecture. Use an {{site.data.keyword.cloud_notm}} [API key](/docs/account?topic=account-userapikey&interface=terraform#create_user_key-api-terra). For more information, see [Ensuring that the API key or infrastructure credentials owner has the correct permissions](/docs/openshift?topic=openshift-access-creds) in the {{site.data.keyword.redhat_openshift_notm}} Cloud docs.

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
